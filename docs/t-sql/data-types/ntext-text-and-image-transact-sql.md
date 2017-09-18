---
title: ntext, text et image (Transact-SQL) | Documents Microsoft
ms.custom: 
ms.date: 07/22/2017
ms.prod: sql-non-specified
ms.reviewer: 
ms.suite: 
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- ntext_TSQL
- ntext
dev_langs:
- TSQL
helpviewer_keywords:
- text data type, about text data type
- text [SQL Server], data types
- ntext data type
- ntext data type, about ntext data type
- image data type, about image data type
ms.assetid: b0d8769c-7598-4f97-8162-ace5f182b5bc
caps.latest.revision: 34
author: BYHAM
ms.author: rickbyh
manager: jhubbard
ms.translationtype: MT
ms.sourcegitcommit: 876522142756bca05416a1afff3cf10467f4c7f1
ms.openlocfilehash: 76b78b01596b484bc35df1a1e468c5b2bb8ece63
ms.contentlocale: fr-fr
ms.lasthandoff: 09/01/2017

---
# <a name="ntext-text-and-image-transact-sql"></a>Types ntext, text et image (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Types de données de longueur fixe et variable, permettant de stocker un grand nombre de caractères Unicode et non-Unicode, ainsi que des données binaires. Les données Unicode utilisent le jeu de caractères UNICODE UCS-2.
  
>**IMPORTANT !**  **ntext**, **texte**, et **image** des types de données seront supprimées dans une future version de SQL Server. Évitez d'utiliser ces types de données dans un nouveau développement. Prévoyez de modifier les applications qui les utilisent actuellement. Utilisez plutôt les types de données [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)et [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) .  
  
  
## <a name="arguments"></a>Arguments  
**ntext**  
Données Unicode de longueur variable ne pouvant pas dépasser 2^30 - 1 octets (c'est-à-dire 1 073 741 823). La taille de stockage, en octets, est le double de la longueur de chaîne entrée. Le synonyme ISO de **ntext** est **texte national**.
  
**text**  
Données non-Unicode de longueur variable figurant dans la page de codes du serveur et ne pouvant pas dépasser en longueur 2^31 - 1 octets (2 147 483 647). Lorsque la page de codes du serveur utilise des caractères sur deux octets, le stockage est tout de même de 2 147 483 647 octets. En fonction de la chaîne de caractères, la taille de stockage peut être inférieure à 2 147 483 647 octets.
  
**image**  
Données binaires de longueur variable occupant de 0 à 2^31 - 1 (2 147 483 647) octets.
  
## <a name="remarks"></a>Notes  
Les fonctions et les instructions suivantes peuvent être utilisées avec **ntext**, **texte**, ou **image** données.
  
|Fonctions|Instructions|  
|---|---|
|[DATALENGTH &#40; Transact-SQL &#41;](../../t-sql/functions/datalength-transact-sql.md)|[READTEXT &#40; Transact-SQL &#41;](../../t-sql/queries/readtext-transact-sql.md)|  
|[La fonction PATINDEX &#40; Transact-SQL &#41;](../../t-sql/functions/patindex-transact-sql.md)|[SET TEXTSIZE &#40; Transact-SQL &#41;](../../t-sql/statements/set-textsize-transact-sql.md)|  
|[SOUS-chaîne &#40; Transact-SQL &#41;](../../t-sql/functions/substring-transact-sql.md)|[UPDATETEXT &#40; Transact-SQL &#41;](../../t-sql/queries/updatetext-transact-sql.md)|  
|[TEXTPTR &#40; Transact-SQL &#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)|[WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)|  
|[TEXTVALID &#40; Transact-SQL &#41;](../../t-sql/functions/text-and-image-functions-textvalid-transact-sql.md)||  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Conversion de Type de données &#40; moteur de base de données &#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[COMME &#40; Transact-SQL &#41;](../../t-sql/language-elements/like-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)

