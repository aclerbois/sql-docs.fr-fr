---
title: -- (Comment) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- --_TSQL
- Comment
- --
dev_langs:
- TSQL
helpviewer_keywords:
- nonexecuting text strings [SQL Server]
- remarks [SQL Server]
- -- (comment character)
- comments [SQL Server]
ms.assetid: 676ea8c2-52c1-4ef6-9354-320f1a091153
caps.latest.revision: 43
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: bbd0746b2c698b0dad4b30580ab0533187d64d12
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="---comment-transact-sql"></a>'-- (Comment) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Indique un texte défini par l'utilisateur. Il est possible d'insérer des commentaires sur une ligne distincte, imbriqués à la fin d'une ligne de commande [!INCLUDE[tsql](../../includes/tsql-md.md)] ou à l'intérieur d'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]. Le serveur n'évalue pas ces commentaires.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
-- text_of_comment  
```  
  
## <a name="arguments"></a>Arguments  
 *text_of_comment*  
 Chaîne de caractères contenant le texte du commentaire.  
  
## <a name="remarks"></a>Notes   
 Utilisez deux tirets (--) pour un commentaire d'une seule ligne ou imbriqué. Les commentaires insérés à l'aide de -- se terminent par le caractère de saut de ligne. Il n'y a pas de longueur maximale pour les commentaires. Le tableau suivant répertorie les raccourcis clavier que vous pouvez utiliser pour commenter du texte ou annuler les marques de commentaire de ce dernier.  
  
|Action|Standard|  
|------------|--------------|  
|Mettre le texte sélectionné en commentaire|Ctrl+K, Ctrl+C|  
|Ne pas commenter le texte sélectionné|Ctrl+K, Ctrl+U|  
  
 Pour plus d’informations sur les raccourcis clavier, consultez [Raccourcis clavier dans SQL Server Management Studio](../../tools/sql-server-management-studio/sql-server-management-studio-keyboard-shortcuts.md).  
  
 Pour les commentaires multilignes, consultez [Barre oblique étoile &#40;bloc de commentaire&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/slash-star-comment-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise les caractères d'indication de commentaire --.  
  
```  
-- Choose the AdventureWorks2012 database.  
USE AdventureWorks2012;  
GO  
-- Choose all columns and all rows from the Address table.  
SELECT *  
FROM Person.Address  
ORDER BY PostalCode ASC; -- We do not have to specify ASC because   
-- that is the default.  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Langage de contrôle de flux &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)  
  
  
