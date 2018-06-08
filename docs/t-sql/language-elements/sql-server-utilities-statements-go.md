---
title: GO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server (starting with 2008)
f1_keywords:
- GO
- GO_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- batches [SQL Server], ending
- ending batches [SQL Server]
- GO command
ms.assetid: b2ca6791-3a07-4209-ba8e-2248a92dd738
caps.latest.revision: 39
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d84d2f8ae7326e610f140a5f0ae2ed20a2e2e1a4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-utilities-statements---go"></a>Instructions d’utilitaires SQL Server - GO
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit des commandes qui ne sont pas des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)], mais qui sont reconnues par les utilitaires **sqlcmd** et **osql**, ainsi que l’éditeur de code [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Ces commandes facilitent la lisibilité et l'exécution de lots et de scripts.  
  
  GO signale la fin d’un lot d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] aux utilitaires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
GO [count]  
```  
  
## <a name="arguments"></a>Arguments  
 *nombre*  
 Entier positif. Le lot qui précède GO sera exécuté le nombre spécifié de fois.  
  
## <a name="remarks"></a>Notes   
 GO n’est pas une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)], mais une commande reconnue par les utilitaires **sqlcmd** et **osql**, ainsi que l’éditeur de code [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Les utilitaires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interprètent GO comme le signal qu'ils doivent envoyer le lot actuel d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le traitement en cours se compose de toutes les transactions entrées depuis la dernière commande GO ou depuis le début de la session ou du script approprié s'il s'agit de la première commande GO.  
  
 Une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] ne peut pas se trouver sur la même ligne qu'une commande GO. Toutefois, la ligne peut contenir des commentaires.  
  
 Les utilisateurs doivent se conformer aux règles en vigueur concernant les traitements. Par exemple, toute exécution d'une procédure stockée faisant suite à la première instruction d'un lot doit inclure le mot clé EXECUTE. La portée des variables locales (personnalisées) se limite à un traitement et il est impossible d'y faire référence après une commande GO.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @MyMsg VARCHAR(50)  
SELECT @MyMsg = 'Hello, World.'  
GO -- @MyMsg is not valid after this GO ends the batch.  
  
-- Yields an error because @MyMsg not declared in this batch.  
PRINT @MyMsg  
GO  
  
SELECT @@VERSION;  
-- Yields an error: Must be EXEC sp_who if not first statement in   
-- batch.  
sp_who  
GO  
```  
  
 Les applications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent envoyer plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en vue de leur exécution sous forme de traitement. Les instructions du traitement sont ensuite compilées dans un plan d'exécution unique. Les programmeurs qui exécutent des instructions appropriées dans les utilitaires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou élaborent des scripts d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] à exécuter par le biais des utilitaires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisent la commande GO pour signaler la fin d'un traitement.  
  
 Les applications basées sur ODBC ou sur les API OLE DB reçoivent une erreur de syntaxe s'ils tentent d'exécuter une commande GO. Les utilitaires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'envoient jamais de commande GO au serveur.  
  
 N'utilisez pas un point-virgule comme terminateur d'instruction après GO.  
  
## <a name="permissions"></a>Autorisations  
 GO est une commande d'utilitaire qui ne nécessite pas d'autorisation. Elle peut être exécutée par tout utilisateur.  
  
```  
-- Yields an error because ; is not permitted after GO  
SELECT @@VERSION;  
GO;  
```  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée deux traitements. Le premier lot contient uniquement une instruction `USE AdventureWorks2012` pour définir le contexte de la base de données. Les autres instructions utilisent une variable locale. Ainsi, toutes les déclarations de variables locales doivent être groupées en un seul traitement. Pour cela, aucune commande `GO` n'est utilisée avant la dernière instruction qui fait référence à la variable.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @NmbrPeople int  
SELECT @NmbrPeople = COUNT(*)  
FROM Person.Person;  
PRINT 'The number of people as of ' +  
      CAST(GETDATE() AS char(20)) + ' is ' +  
      CAST(@NmbrPeople AS char (10));  
GO  
```  
  
 L'exemple suivant exécute les instructions dans le lot à deux reprises.  
  
```  
SELECT DB_NAME();  
SELECT USER_NAME();  
GO 2  
```  
  
  
