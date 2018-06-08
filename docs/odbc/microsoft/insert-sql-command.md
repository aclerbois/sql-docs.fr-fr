---
title: INSERT - commande SQL | Documents Microsoft
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
- INSERT [ODBC]
ms.assetid: 9b648198-349f-46f6-b869-13d129945971
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 881ee4d74eab6b1e26b1f3ec243a39c08125ea00
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="insert---sql-command"></a>INSERT - commande SQL
Ajoute un enregistrement à la fin d’une table qui contient les valeurs du champ spécifié.  
  
 Le pilote ODBC Visual FoxPro prend en charge la syntaxe du langage Visual FoxPro native pour cette commande. Pour plus d’informations spécifiques au pilote, consultez la section Notes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
INSERT INTO dbf_name [(fname1 [, fname2, ...])]  
   VALUES (eExpression1 [, eExpression2, ...])  
```  
  
## <a name="arguments"></a>Arguments  
 INSERT INTO *dbf_name*  
 Spécifie le nom de la table à laquelle le nouvel enregistrement est ajouté. *dbf_name* peut inclure un chemin d’accès et un nom d’expression.  
  
 Si la table que vous spécifiez n’est pas ouverte, il est ouvert en mode exclusif dans une nouvelle zone de travail et le nouvel enregistrement est ajouté à la table. La nouvelle zone de travail n’est pas sélectionnée ; l’espace de travail actuel est sélectionné.  
  
 Si la table que vous spécifiez est ouverte, INSERT ajoute le nouvel enregistrement à la table. Si la table est ouverte dans une zone de travail autres que la zone de travail en cours, il n’est pas activée après l’ajout de l’enregistrement ; l’espace de travail actuel est sélectionné.  
  
 [( *fname1*[, *fname2*[,...]])]  
 Spécifie, dans le nouvel enregistrement, les noms des champs dans lesquels les valeurs sont insérées.  
  
 VALEURS ( *eExpression1*[, *eExpression2*[,...]])  
 Spécifie les valeurs de champ insérés dans le nouvel enregistrement. Si vous omettez les noms de champ, vous devez spécifier les valeurs de champ dans l’ordre défini par la structure de table.  
  
## <a name="remarks"></a>Notes  
 Le nouvel enregistrement contient les données figurant dans la clause VALUES.  
  
## <a name="driver-remarks"></a>Section Notes de pilote  
 Lorsque votre application envoie l’instruction SQL ODBC INSERT à la source de données, le pilote ODBC Visual FoxPro convertit la commande dans la commande Visual FoxProINSERT sans traduction.  
  
## <a name="see-also"></a>Voir aussi  
 [CRÉER la TABLE - commande SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [SELECT, commande SQL](../../odbc/microsoft/select-sql-command.md)
