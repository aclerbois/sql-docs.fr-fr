---
title: APP_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- APP_NAME_TSQL
- APP_NAME
dev_langs:
- TSQL
helpviewer_keywords:
- name checking for current session [SQL Server]
- sessions [SQL Server], application names
- applications [SQL Server], names
- current session application names
- APP_NAME function
ms.assetid: e491e192-9b30-4243-bc19-33c133fe08a8
caps.latest.revision: 35
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 9d9652c25875fb1edb5d09e4e283fd7393b523a6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="appname-transact-sql"></a>APP_NAME (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Fonction qui retourne le nom de l’application pour la session en cours, si l’application définit la valeur de ce nom.
  
> [!IMPORTANT]  
>  Le client fournit le nom de l’application et la valeur de ce nom n’est vérifiée en aucune façon. N’utilisez pas **APP_NAME** dans le cadre d’une vérification de sécurité.  
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
  
APP_NAME  ( )  
```  
  
## <a name="return-types"></a>Types de retour  
**nvarchar(128)**
  
## <a name="remarks"></a>Notes   
Utilisez **APP_NAME** pour faire la distinction entre différentes applications, comme un moyen d’effectuer des actions différentes pour ces applications. Par exemple, **APP_NAME** peut faire la distinction entre les différentes applications pour autoriser un format de date différent pour chaque application. Elle peut également autoriser le renvoi d’un message d’information à certaines applications.
  
Pour définir un nom d’application dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], cliquez sur **Options** dans la boîte de dialogue **Se connecter au moteur de base de données**. Sous l’onglet **Paramètres de connexion supplémentaires**, spécifiez un attribut **app** au format `;app='application_name'`
  
## <a name="example"></a> Exemple  
Cet exemple vérifie si l'application cliente qui a lancé ce processus est une session `SQL Server Management Studio` et fournit une date au format US ou ANSI.
  
```sql
USE AdventureWorks2012;  
GO  
IF APP_NAME() = 'Microsoft SQL Server Management Studio - Query'  
PRINT 'This process was started by ' + APP_NAME() + '. The date is ' + CONVERT ( varchar(100) , GETDATE(), 101) + '.';  
ELSE   
PRINT 'This process was started by ' + APP_NAME() + '. The date is ' + CONVERT ( varchar(100) , GETDATE(), 102) + '.';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi
[Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-for-transact-sql.md)  
[Fonctions](../../t-sql/functions/functions.md)
  
  
