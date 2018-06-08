---
title: Commande SET COLLATE | Documents Microsoft
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
- set collate command [ODBC]
ms.assetid: 00efbcd4-fea8-4061-86a5-82de413cb753
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f1ce3b75134da55286f40dc77cf6f6ba7de7e5b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-collate-command"></a>Commande SET COLLATE
Spécifie une séquence de classement pour les champs de type caractère dans l’indexation suivantes et les opérations de tri.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET COLLATE TO cSequenceName  
```  
  
## <a name="arguments"></a>Arguments  
 *cSequenceName*  
 Spécifie une séquence de classement. Les options de séquence de classement disponibles sont décrits dans le tableau suivant.  
  
|Options|Langage|  
|-------------|--------------|  
|NÉERLANDAIS|Néerlandais|  
|GENERAL|Anglais, Français, allemand, Espagnol moderne, portugais et autres langues d’Europe occidentale|  
|ALLEMAND|Ordre allemand annuaire téléphonique (DIN)|  
|ISLANDE|Islandais|  
|ORDINATEUR|Machine (la séquence de classement par défaut pour les versions antérieures de FoxPro)|  
|NORDAN|Norvégien, danois|  
|ESPAGNOL|Espagnol|  
|SWEFIN|Suédois, finnois|  
|UNIQWT|Poids unique|  
  
> [!NOTE]  
>  Lorsque vous spécifiez l’option espagnole, *ch* est une lettre unique qui trie entre *c* et *d*, et *ll* trie entre *l* et *m*.  
  
 Si vous spécifiez une option de séquence de classement en tant que caractère littéral chaîne, veillez à placer l’option de guillemets :  
  
```  
SET COLLATE TO "SWEFIN"  
```  
  
 MACHINE est l’option de séquence de classement par défaut et que les utilisateurs Xbase séquence sont familiarisés avec. Caractères sont classés comme ils apparaissent dans la page de codes actuelle.  
  
 GÉNÉRAL peut être préférable pour les utilisateurs des États-Unis et Europe. Caractères sont classés comme ils apparaissent dans la page de codes actuelle. Dans les versions FoxPro antérieures à 2.5, les index peuvent avoir été créés à l’aide de la **supérieur**() ou **inférieure**fonctions () pour convertir les champs de type caractère à un incident cohérent. Dans les versions FoxPro 2.5 plus tard, vous pouvez à la place spécifier l’option de séquence de classement général et omettre les **supérieur**conversion de ().  
  
 Si vous spécifiez une option de séquence de classement autre que l’ordinateur et si vous créez un fichier .idx, un .idx compact est toujours créé.  
  
 Utilisez SET("COLLATE") pour retourner la séquence de classement actuel.  
  
 Vous pouvez spécifier un ordre de classement pour une source de données à l’aide de la [boîte de dialogue d’installation ODBC Visual FoxPro](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md) ou à l’aide du mot clé Collate dans votre chaîne de connexion avec [SQLDriverConnect](../../odbc/microsoft/sqldriverconnect-visual-foxpro-odbc-driver.md). Cela est identique à l’émission de la commande suivante :  
  
```  
SET COLLATE TO cSequenceName  
```  
  
## <a name="remarks"></a>Notes  
 VALEUR COLLATE vous permet d’ordonner les tables contenant les caractères accentués pour toutes les langues prises en charge. Modification du paramètre de valeur COLLATE n’affecte pas l’ordre de classement des index précédemment ouverts. Visual FoxPro gère automatiquement les index existants, permettant de créer différents types d’index, même pour le même champ.  
  
 Par exemple, si un index est créé avec la valeur COLLATE général la valeur et le paramètre de valeur COLLATE est modifié en espagnol, l’index conserve la séquence de classement général.  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration d’ODBC pour Visual FoxPro, boîte de dialogue](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md)
