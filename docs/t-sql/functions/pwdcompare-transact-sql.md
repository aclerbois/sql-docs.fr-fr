---
title: PWDCOMPARE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- PWDCOMPARE
- PWDCOMPARE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sa account
- passwords [SQL Server], blank
- PWDCOMPARE function [Transact-SQL]
ms.assetid: 5f84ff9e-c1ec-46aa-8501-50f854ebcc3a
caps.latest.revision: 15
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: ad1e978fd88943aef2f6e2b0bed4bf98f1a96ce0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pwdcompare-transact-sql"></a>PWDCOMPARE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Hache un mot de passe et compare le hachage au hachage d'un mot de passe existant. PWDCOMPARE peut être utilisé pour rechercher les mots de passe de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vides ou les mots de passe faibles courants.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
PWDCOMPARE ( 'clear_text_password'  
   , password_hash   
   [ , version ] )  
```  
  
## <a name="arguments"></a>Arguments  
 **'** *clear_text_password* **'**  
 Mot de passe non chiffré. *clear_text_password* est de type **sysname** (**nvarchar(128)**).  
  
 *password_hash*  
 Hachage de chiffrement d'un mot de passe. *password_hash* est de type **varbinary(128)**.  
  
 *version*  
 Paramètre obsolète auquel la valeur 1 peut être affectée si *password_hash* représente une valeur d’un compte de connexion antérieur à [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] qui a été migrée vers [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou version ultérieure, mais n’a jamais été converti vers le système [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. *version* est de type **int**.  
  
> [!CAUTION]  
>  Ce paramètre est fourni pour des raisons de compatibilité descendante, mais est ignoré car les objets blob de hachage de mot de passe contiennent maintenant leurs propres descriptions de version. [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)]  
  
## <a name="return-types"></a>Types de retour  
 **Int**  
  
 Renvoie 1 si le hachage de *clear_text_password* correspond au paramètre *password_hash*, et 0 si ce n’est pas le cas.  
  
## <a name="remarks"></a>Notes   
 La fonction PWDCOMPARE ne constitue pas une menace pour la force des hachages de mot de passe car le même test pourrait être effectué en essayant de se connecter à l'aide du mot de passe fourni en tant que premier paramètre.  
  
 **PWDCOMPARE** ne peut pas être utilisé avec les mots de passe des utilisateurs de bases de données à relation contenant-contenu. Il n'existe aucun équivalent de base de données à relation contenant-contenu.  
  
## <a name="permissions"></a>Autorisations  
 PWDENCRYPT est accessible publiquement.  
  
 L'autorisation CONTROL SERVER est requise pour examiner la colonne password_hash de sys.sql_logins.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-identifying-logins-that-have-no-passwords"></a>A. Identification de comptes de connexion qui n'ont pas de mots de passe  
 L'exemple suivant identifie les comptes de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui n'ont pas de mots de passe.  
  
```  
SELECT name FROM sys.sql_logins   
WHERE PWDCOMPARE('', password_hash) = 1 ;  
```  
  
### <a name="b-searching-for-common-passwords"></a>B. Recherche de mots de passe communs  
 Pour rechercher les mots de passe communs que vous souhaitez identifier et modifier, spécifiez le mot de passe en tant que premier paramètre. Par exemple, exécutez l'instruction suivante pour rechercher un mot de passe spécifié comme `password`.  
  
```  
SELECT name FROM sys.sql_logins   
WHERE PWDCOMPARE('password', password_hash) = 1 ;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [PWDENCRYPT &#40;Transact-SQL&#41;](../../t-sql/functions/pwdencrypt-transact-sql.md)   
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
