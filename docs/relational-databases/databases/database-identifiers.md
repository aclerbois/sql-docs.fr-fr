---
title: Identificateur de la base de données | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- regular identifiers [SQL Server]
- identifiers [SQL Server]
- names [SQL Server], identifiers
- identifiers [SQL Server], about identifiers
- SQL Server identifiers
- Transact-SQL identifiers
- database objects [SQL Server], names
ms.assetid: 171291bb-f57f-4ad1-8cea-0b092d5d150c
caps.latest.revision: 40
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: ea619fe016adff016241964c605e4db6c3c70377
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="database-identifiers"></a>Identificateur de la base de données
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Le nom d'un objet d'une base de données est son identificateur. Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tous les éléments peuvent avoir un identificateur. Les serveurs, les bases de données et les objets de bases de données tels que les tables, les vues, les colonnes, les index, les déclencheurs, les procédures, les contraintes, les règles, etc. peuvent avoir des identificateurs. La plupart des objets doivent avoir un identificateur ; les identificateurs sont facultatifs pour certains objets, tels que les contraintes.  
  
 L'identificateur d'un objet est créé lors de la définition de l'objet. L'identificateur est ensuite utilisé pour référencer l'objet. L'instruction suivante, par exemple, crée une table avec l'identificateur `TableX`, et deux colonnes avec les identificateurs `KeyCol` et `Description`:  
  
```  
CREATE TABLE TableX  
(KeyCol INT PRIMARY KEY, Description nvarchar(80))  
```  
  
 Cette table comporte également une contrainte sans nom. La contrainte `PRIMARY KEY` n'a pas d'identificateur.  
  
 Le classement d'un identificateur dépend du niveau auquel il est défini. Le classement par défaut de l'instance est assigné aux identificateurs d'objets qui sont au niveau de l'instance, tels que les noms de connexion et de base de données. Le classement par défaut de la base de données est affecté aux identificateurs d'objets qui appartiennent à la base de données, tels que les noms des tables, des vues et des colonnes. Par exemple, deux tables dont les noms diffèrent uniquement au niveau de la casse peuvent être créées dans une base de données dont le classement respecte la casse, mais pas dans une base de données dont le classement ne respecte pas la casse.  
  
> [!NOTE]  
>  Les noms de variables, ou les paramètres des fonctions et des procédures stockées doivent toujours respecter les règles des identificateurs [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
## <a name="classes-of-identifiers"></a>Classes d'identificateurs  
 Il existe deux classes d'identificateurs :  
  
 Identificateurs réguliers  
 Les identificateurs réguliers respectent les règles relatives au format des identificateurs. Ils ne sont pas délimités lorsqu'ils sont utilisés dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
```  
SELECT *  
FROM TableX  
WHERE KeyCol = 124  
```  
  
 Identificateurs délimités  
 Les identificateurs délimités sont mis entre guillemets (") ou entre crochets ([ ]). Les identificateurs qui respectent les règles relatives au format des identificateurs peuvent ne pas être délimités. Exemple :  
  
```  
SELECT *  
FROM [TableX]         --Delimiter is optional.  
WHERE [KeyCol] = 124  --Delimiter is optional.  
```  
  
 Ceux qui ne respectent pas ces règles ne peuvent être utilisés dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qu'en étant délimités. Exemple :  
  
```  
SELECT *  
FROM [My Table]      --Identifier contains a space and uses a reserved keyword.  
WHERE [order] = 10   --Identifier is a reserved keyword.  
```  
  
 Qu'ils soient réguliers ou délimités, les identificateurs doivent contenir de 1 à 128 caractères. Dans le cas des tables temporaires locales, l'identificateur peut contenir jusqu'à 116 caractères.  
  
## <a name="rules-for-regular-identifiers"></a>Règles pour identificateurs réguliers  
 Les noms de variables, de fonctions et de procédures stockées doivent toujours respecter les règles suivantes portant sur les identificateurs [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
1.  Le premier caractère doit être l'un des suivants :  
  
    -   Une des lettres définies par Unicode Standard 3,2. Elles incluent les caractères latins de a à z et de A à Z ainsi que des caractères alphabétiques d'autres langues.  
  
    -   Les symboles trait de soulignement (_), arobase (@) ou dièse (#).  
  
         Certains symboles au début d'un identificateur ont une signification particulière dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un identificateur régulier qui commence par le signe arobase (@) dénote toujours une variable ou un paramètre local et ne peut pas être utilisé comme le nom d'un autre type d'objet. Un identificateur commençant par un symbole numéro indique un objet temporaire (table ou procédure). Un identificateur commençant par le double signe # (##) indique un objet temporaire global. Bien que les symboles dièse (#) et double dièse (##) puissent être utilisés pour commencer les noms d'autres types d'objets, nous ne recommandons pas cette pratique.  
  
         Le nom de certaines fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] commence par un double arobas (@@). Pour éviter toute confusion avec ces fonctions, n’utilisez pas de noms commençant par @@.  
  
2.  Les caractères suivants peuvent inclure les éléments suivants :  
  
    -   Des lettres définies dans Unicode Standard 3,2.  
  
    -   Des nombres décimaux de Basic Latin ou d'autres scripts nationaux.  
  
    -   L'arobase, le symbole dollar ($), le symbole dièse ou le trait de soulignement.  
  
3.  L'identificateur ne doit pas être un mot réservé [!INCLUDE[tsql](../../includes/tsql-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve les majuscules et les minuscules des mots réservés. Un identificateur qui ne respecte pas toutes ces règles doit toujours être délimité par des crochets ou des guillemets doubles lors de son utilisation dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] . Les mots réservés dépendent du niveau de compatibilité de la base de données. Vous pouvez définir ce niveau avec l’instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) .  
  
4.  Les espaces incorporés ou les caractères spéciaux ne sont pas autorisés.  
  
5.  L'utilisation de caractères supplémentaires n'est pas autorisée.  
  
 Un identificateur qui ne respecte pas toutes ces règles doit toujours être délimité par des crochets ou des guillemets doubles lors de son utilisation dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
> [!NOTE]  
>  Certaines règles relatives au format des identificateurs réguliers dépendent du niveau de compatibilité de la base de données. Vous pouvez définir ce niveau avec [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Mots clés réservés &#40;Transact-SQL&#41;](../../t-sql/language-elements/reserved-keywords-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  
  
  
