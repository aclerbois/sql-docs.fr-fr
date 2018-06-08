---
title: DROP DEFAULT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DROP_DEFAULT_TSQL
- DROP DEFAULT
dev_langs:
- TSQL
helpviewer_keywords:
- DROP DEFAULT statement
- defaults [SQL Server], removing
ms.assetid: d2d3af25-8877-46ba-95d9-1844961d97ee
caps.latest.revision: 43
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 1f545dbf2be3e9c1e741373bf5c04abd34c140e7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="drop-default-transact-sql"></a>DROP DEFAULT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime une ou plusieurs valeurs par défaut définies par l'utilisateur de la base de données active.  
  
> [!IMPORTANT]  
>  La fonction DROP DEFAULT sera retirée dans la prochaine version de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez de l'utiliser dans tout nouveau travail de développement et prévoyez la modification des applications qui s'en servent actuellement. À la place, utilisez des définitions par défaut que vous pouvez créer en utilisant le mot clé DEFAULT de [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou de [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
DROP DEFAULT [ IF EXISTS ] { [ schema_name . ] default_name } [ ,...n ] [ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 *IF EXISTS*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Supprime, de manière conditionnelle, la valeur par défaut uniquement si elle existe déjà.  
  
 *schema_name*  
 Nom du schéma auquel appartient la valeur par défaut.  
  
 *default_name*  
 Nom de la valeur par défaut existante. Pour afficher la liste des valeurs par défaut qui existent, exécutez **sp_help**. Les valeurs par défaut doivent respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md). La définition du nom de schéma par défaut est facultative.  
  
## <a name="remarks"></a>Notes   
 Avant de supprimer une valeur par défaut, supprimez sa liaison en exécutant **sp_unbindefault** si la valeur par défaut est actuellement liée à une colonne ou à un type de données alias.  
  
 Après avoir supprimé une valeur par défaut d'une colonne qui autorise les valeurs NULL, NULL est inséré dans cette position lorsque des lignes sont ajoutées et qu'aucune valeur n'est explicitement fournie. Après avoir supprimé une valeur par défaut dans une colonne NOT NULL, vous recevez un message d'erreur lorsque vous tentez d'ajouter des lignes sans fournir explicitement une valeur. Ces lignes sont ajoutées ultérieurement comme composantes du comportement général de l'instruction INSERT.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter DROP DEFAULT, l'utilisateur doit avoir l'autorisation ALTER sur le schéma auquel la valeur par défaut appartient.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-dropping-a-default"></a>A. Suppression d'une valeur par défaut  
 Si une valeur par défaut n'est pas liée à une colonne ou à un type de données alias, vous pouvez la supprimer en utilisant DROP DEFAULT. L'exemple suivant supprime la valeur par défaut `datedflt` créée par l'utilisateur.  
  
```  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.objects  
         WHERE name = 'datedflt'   
            AND type = 'D')  
   DROP DEFAULT datedflt;  
GO  
```  
  
 Depuis [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], vous pouvez utiliser la syntaxe suivante.  
  
```  
DROP DEFAULT IF EXISTS datedflt;  
GO  
```  
  
### <a name="b-dropping-a-default-that-has-been-bound-to-a-column"></a>B. Suppression d'une valeur par défaut liée à une colonne  
 L'exemple suivant dissocie la valeur par défaut associée à la colonne `EmergencyContactPhone` de la table `Contact` et supprime la valeur par défaut `phonedflt`.  
  
```  
USE AdventureWorks2012;  
GO  
   BEGIN   
      EXEC sp_unbindefault 'Person.Contact.Phone'  
      DROP DEFAULT phonedflt  
   END;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)  
  
  
