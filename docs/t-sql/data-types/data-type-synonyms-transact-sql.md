---
title: Synonymes des types de données (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 7/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], synonyms
- alternate names [SQL Server]
- synonyms [SQL Server], data types
ms.assetid: 390eef67-1a49-4185-a971-e07765be9717
caps.latest.revision: 18
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 4b0e16289f244a8b1fdf24bb3b41e253a1d70c5f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-type-synonyms-transact-sql"></a>Synonymes des types de données (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

Les synonymes des types de données sont inclus dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la conformité aux normes ISO. Le tableau suivant répertorie les synonymes et les types de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auxquels ils sont mappés.
  
|Synonyme|Type de données système SQL Server|  
|---|---|
|**binary varying**|**varbinary**|  
|**char varying**|**varchar**|  
|**character**|**char**|  
|**character**|**char(1)**|  
|**character(** *n* **)**|**char(n)**|  
|**character varying(** *n* **)**|**varchar(n)**|  
|**Dec**|**decimal**|  
|**Double precision**|**float**|  
|**float**[**(***n***)**] for *n* = 1-7|**real**|  
|**float**[**(***n***)**] for *n* = 8-15|**float**|  
|**entier**|**Int**|  
|**national character(** *n* **)**|**nchar(n)**|  
|**national char(** *n* **)**|**nchar(n)**|  
|**national character varying(** *n* **)**|**nvarchar(n)**|  
|**national char varying(** *n* **)**|**nvarchar(n)**|  
|**national text**|**ntext**|  
|**timestamp**|rowversion|  
  
Les synonymes des types de données peuvent être utilisés à la place du nom du type de données de base correspondant dans les instructions DDL (Data Definition Language), telles que CREATE TABLE, CREATE PROCEDURE ou DECLARE *@variable*. Cependant, la visibilité des synonymes est nulle après la création de l'objet. Une fois l'objet créé, il reçoit le type de données de base associé au synonyme. La consignation ne spécifie pas que le synonyme a été utilisé dans l'instruction ayant créé l'objet.
  
Tous les objets issus de l'objet d'origine, tels que les expressions ou colonnes de l'ensemble de résultats, reçoivent le type de données de base. Toutes les fonctions de métadonnées ultérieurement appliquées à l'objet d'origine et tous les objets dérivés font état du type de données de base, non du synonyme. Ce comportement se produit avec les opérations sur les métadonnées, telles que **sp_help** et autres procédures stockées système, les vues de schémas d’informations ou les différentes opérations API d’accès aux données faisant état des types de données des colonnes de l’ensemble de résultats ou de table.
  
Vous pouvez, par exemple, créer une table en spécifiant `national character varying` :
  
```sql
CREATE TABLE ExampleTable (PriKey int PRIMARY KEY, VarCharCol national character varying(10))  
```  
  
`VarCharCol` reçoit en réalité un type de données **nvarchar(10)** et toutes les fonctions de métadonnées ultérieures signalent la colonne comme étant **nvarchar(10)**. Les fonctions de métadonnées ne les signaleront jamais comme colonne **national character varying(10)**.
  
## <a name="see-also"></a>Voir aussi
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
  
