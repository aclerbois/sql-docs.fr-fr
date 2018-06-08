---
title: SUSER_SID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SUSER_SID
- SUSER_SID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- logins [SQL Server], users
- SIDs [SQL Server]
- security identifiers [SQL Server]
- users [SQL Server], logins
- 15401 (Database Engine error)
- IDs [SQL Server], logins
- identification numbers [SQL Server], logins
- SUSER_SID function
ms.assetid: 57b42a74-94e1-4326-85f1-701b9de53c7d
caps.latest.revision: 31
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a1d39082b0baa47577fb4315a7b16d182b39b3ae
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34585801"
---
# <a name="susersid-transact-sql"></a>SUSER_SID (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Renvoie le numéro d'identification de sécurité (SID) correspondant au nom de connexion spécifié.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SUSER_SID ( [ 'login' ] [ , Param2 ] )   
```  
  
## <a name="arguments"></a>Arguments  
 **'** *login* **'**  
**S’applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]
  
 Nom de connexion de l'utilisateur. *login* est de type **sysname**. Cet argument facultatif *login* peut correspondre à une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à un groupe ou utilisateur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Si *login* n’est pas spécifié, des informations sur le contexte de sécurité actuel sont retournées. Si le paramètre contient le mot NULL, retourne NULL.  
  
 *Param2*  
**S’applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]
  
 Indique si le nom de connexion est validé. *Param2* est de type **int** et est facultatif. Lorsque *Param2* a la valeur 0, le nom de connexion n’est pas validé. Lorsque *Param2* n’est pas spécifié avec la valeur 0, une vérification est effectuée pour s’assurer que le nom de connexion Windows est exactement le même que le nom de connexion stocké dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-types"></a>Types de retour  
 **varbinary(85)**  
  
## <a name="remarks"></a>Notes   
 La fonction SUSER_SID peut être utilisée comme une contrainte DEFAULT dans les fonctions ALTER TABLE ou CREATE TABLE. SUSER_SID peut être utilisé dans la liste SELECT, dans une clause WHERE, et partout où une expression est autorisée. SUSER_SID doit toujours être suivi de parenthèses, même si aucun paramètre n'est spécifié.  
  
 Lorsque la procédure SUSER_SID est appelée sans argument, elle renvoie l'ID de sécurité (SID) du contexte de sécurité actuel. Lorsqu'elle est appelée sans argument dans un lot qui a changé le contexte à l'aide de l'instruction EXECUTE AS, elle retourne le SID du contexte dont l'identité a été empruntée. Lorsqu'elle est appelée à partir d'un contexte faisant l'objet d'un emprunt d'identité, SUSER_SID(ORIGINAL_LOGIN()) retourne le SID du contexte d'origine.  
  
 Lorsque le classement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le classement Windows sont différents, SUSER_SID peut échouer lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows stockent la connexion dans un format différent. Par exemple, si l'ordinateur Windows TestComputer a la connexion User et que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stocke la connexion sous la forme TESTCOMPUTER\User, la recherche de la connexion TestComputer\User peut ne pas réussir à résoudre correctement le nom de connexion. Pour ignorer cette validation du nom de connexion, utilisez *Param2*. Des classements différents sont souvent à l'origine de l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 15401 :  
  
 `Windows NT user or group '%s' not found. Check the name again.`  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-susersid"></a>A. Utilisation de SUSER_SID  
 L'exemple suivant renvoie le numéro d'identification de sécurité (SID) du contexte de sécurité actuel.  
  
```  
SELECT SUSER_SID();  
```  
  
### <a name="b-using-susersid-with-a-specific-login"></a>B. Utilisation de SUSER_SID avec une connexion spécifique  
 L’exemple suivant retourne le numéro d’identification de sécurité du compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `sa`.  
  
**S’applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]
  
```  
SELECT SUSER_SID('sa');  
GO  
```  
  
### <a name="c-using-susersid-with-a-windows-user-name"></a>C. Utilisation de SUSER_SID avec un nom d'utilisateur Windows  
 L'exemple suivant renvoie le numéro d'identification de sécurité du `London\Workstation1` de l'utilisateur Windows.  
  
**S’applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]
  
```  
SELECT SUSER_SID('London\Workstation1');  
GO  
```  
  
### <a name="d-using-susersid-as-a-default-constraint"></a>D. Utilisation de SUSER_SID comme contrainte DEFAULT  
 L'exemple suivant utilise `SUSER_SID` comme contrainte `DEFAULT` dans une instruction `CREATE TABLE`.  
  
```  
USE AdventureWorks2012;  
GO  
CREATE TABLE sid_example  
(  
login_sid   varbinary(85) DEFAULT SUSER_SID(),  
login_name  varchar(30) DEFAULT SYSTEM_USER,  
login_dept  varchar(10) DEFAULT 'SALES',  
login_date  datetime DEFAULT GETDATE()  
);   
GO  
INSERT sid_example DEFAULT VALUES;  
GO  
```  
  
### <a name="e-comparing-the-windows-login-name-to-the-login-name-stored-in-sql-server"></a>E. Comparaison du nom de connexion Windows et du nom de connexion stocké dans SQL Server  
 L’exemple suivant montre comment utiliser *Param2* pour obtenir le SID de Windows et utilise ce SID comme entrée de la fonction `SUSER_SNAME`. Il indique la connexion au format dans lequel elle est stockée dans Windows (`TestComputer\User`), et retourne la connexion au format dans lequel elle est stockée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (`TESTCOMPUTER\User`).  
  
**S’applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]
  
```  
SELECT SUSER_SNAME(SUSER_SID('TestComputer\User', 0));  
```  
  
## <a name="see-also"></a> Voir aussi  
 [ORIGINAL_LOGIN &#40;Transact-SQL&#41;](../../t-sql/functions/original-login-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [binary et varbinary &#40;Transact-SQL&#41;](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)   
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-for-transact-sql.md)  
  
  
