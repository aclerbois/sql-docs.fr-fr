---
title: FORMATMESSAGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- FORMATMESSAGE
- FORMATMESSAGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmessages system table
- sys.messages catalog view
- FORMATMESSAGE function
- messages [SQL Server], formats
- errors [SQL Server], formats
ms.assetid: 83f18102-2035-4a87-acd0-8d96d03efad5
caps.latest.revision: 39
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: eacacfe4ae5ec32a97ec0ed9f1a9ee33a08d6b52
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="formatmessage-transact-sql"></a>FORMATMESSAGE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Construit un message à partir d’un message existant dans sys.messages ou à partir d’une chaîne fournie. La fonctionnalité de FORMATMESSAGE ressemble à celle de l'instruction RAISERROR. Cependant, RAISERROR imprime immédiatement le message, tandis que FORMATMESSAGE retourne le message mis en forme pour un traitement ultérieur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
FORMATMESSAGE ( { msg_number  | ' msg_string ' } , [ param_value [ ,...n ] ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *msg_number*  
 ID du message stocké dans sys.messages. Si *msg_number* est <= 13000 ou si le message n’existe pas dans sys.messages, la valeur NULL est retournée.  
  
 *msg_string*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Chaîne entre guillemets simples, contenant des espaces réservés de valeurs de paramètres. Le message d'erreur peut compter jusqu'à 2 047 caractères. Si le message contient au moins 2 048 caractères, seuls les 2 044 premiers sont affichés et des points de suspension sont ajoutés pour indiquer que le message a été tronqué. Notez que les paramètres de substitution utilisent plus de caractères que ce que la sortie affiche en raison de son comportement de stockage interne.  Pour obtenir des informations sur la structure d’une chaîne de message et l’utilisation de paramètres dans la chaîne, consultez la description de l’argument *msg_str* dans [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md).  
  
 *param_value*  
 Valeur de paramètre à utiliser dans le message. Vous pouvez utiliser plusieurs valeurs de paramètre. Les valeurs doivent être spécifiées dans l'ordre selon lequel les variables d'espace réservé apparaissent dans le message. Le nombre maximal des valeurs est 20.  
  
## <a name="return-types"></a>Types de retour  
 **nvarchar**  
  
## <a name="remarks"></a>Notes   
 Comme l'instruction RAISERROR, FORMATMESSAGE modifie le message en substituant les valeurs de paramètre fournies par des variables d'espace réservé dans le message. Pour plus d’informations concernant les espaces réservés autorisés dans les messages d’erreur et le processus de modification, consultez [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md).  
  
 FORMATMESSAGE recherche le message dans le langage courant de l'utilisateur. S'il n'existe pas de version localisée du message, la version américaine (U.S. English) est utilisée.  
  
 Pour les messages localisés, les valeurs de paramètres fournies doivent correspondre aux espaces réservés des paramètres de la version américaine. Ainsi, le paramètre 1 dans la version localisée doit correspondre au paramètre 1 dans la version américaine (U.S. English), le paramètre 2 doit correspondre au paramètre 2 de cette dernière, et ainsi de suite.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-example-with-a-message-number"></a>A. Exemple avec un numéro de message  
 L’exemple suivant utilise un message de réplication `20009` stocké dans sys.messages sous la forme « L’article '%s' n’a pas pu être ajouté à la publication '%s' ». FORMATMESSAGE substitue les valeurs `First Variable` et `Second Variable` aux espaces réservés du paramètre. La chaîne finale résultante,« L’article 'First Variable' n’a pas pu être ajouté à la publication 'Second Variable'. », est stockée dans la variable locale `@var1`.  
  
```  
SELECT text FROM sys.messages WHERE message_id = 20009 AND language_id = 1033;  
DECLARE @var1 VARCHAR(200);   
SELECT @var1 = FORMATMESSAGE(20009, 'First Variable', 'Second Variable');   
SELECT @var1;  
```  
  
### <a name="b-example-with-a-message-string"></a>B. Exemple avec une chaîne de message  
  
**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 L’exemple suivant accepte une chaîne en tant qu’entrée.  
  
```  
SELECT FORMATMESSAGE('This is the %s and this is the %s.', 'first variable', 'second variable') AS Result;  
```  
  
 Retourne : `This is the first variable and this is the second variable.`  
  
### <a name="c-additional-message-string-formatting-examples"></a>C. Exemples supplémentaires de mise en forme d’une chaîne de message  
 Les exemples suivants montrent diverses options de mise en forme.  
  
```  
SELECT FORMATMESSAGE('Signed int %i, %d %i, %d, %+i, %+d, %+i, %+d', 5, -5, 50, -50, -11, -11, 11, 11);  
SELECT FORMATMESSAGE('Signed int with leading zero %020i', 5);  
SELECT FORMATMESSAGE('Signed int with leading zero 0 %020i', -55);  
SELECT FORMATMESSAGE('Unsigned int %u, %u', 50, -50);  
SELECT FORMATMESSAGE('Unsigned octal %o, %o', 50, -50);  
SELECT FORMATMESSAGE('Unsigned hexadecimal %x, %X, %X, %X, %x', 11, 11, -11, 50, -50);  
SELECT FORMATMESSAGE('Unsigned octal with prefix: %#o, %#o', 50, -50);  
SELECT FORMATMESSAGE('Unsigned hexadecimal with prefix: %#x, %#X, %#X, %X, %x', 11, 11, -11, 50, -50);  
SELECT FORMATMESSAGE('Hello %s!', 'TEST');  
SELECT FORMATMESSAGE('Hello %20s!', 'TEST');  
SELECT FORMATMESSAGE('Hello %-20s!', 'TEST');  
SELECT FORMATMESSAGE('Hello %20s!', 'TEST');  
```  
  
## <a name="see-also"></a> Voir aussi  
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)  
 [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-for-transact-sql.md)   
  
  
