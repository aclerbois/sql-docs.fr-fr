---
title: CRÉER la TABLE - commande SQL | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- CREATE TABLE [ODBC]
ms.assetid: be2143ba-fc16-42c9-84f7-8985cd924860
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 35a23b8c648b5ffbf2a2000c949f1cb097ba91ca
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-table---sql-command"></a>CRÉER la TABLE - commande SQL
Crée une table comportant les champs spécifiés.  
  
 Le pilote ODBC Visual FoxPro prend en charge la syntaxe du langage Visual FoxPro native pour cette commande. Pour plus d’informations spécifiques au pilote, consultez **pilote notes**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CREATE TABLE | DBF TableName1 [NAME LongTableName] [FREE]  
   (FieldName1FieldType [(nFieldWidth [, nPrecision])]  
      [NULL | NOT NULL]   
      [CHECK lExpression1 [ERROR cMessageText1]]  
      [DEFAULT eExpression1]  
      [PRIMARY KEY | UNIQUE]  
      [REFERENCES TableName2 [TAG TagName1]]  
      [NOCPTRANS]  
   [, FieldName2 ...]  
      [, PRIMARY KEY eExpression2 TAG TagName2  
      |, UNIQUE eExpression3 TAG TagName3]  
      [, FOREIGN KEY eExpression4 TAG TagName4 [NODUP]  
            REFERENCES TableName3 [TAG TagName5]]  
      [, CHECK lExpression2 [ERROR cMessageText2]])  
| FROM ARRAY ArrayName  
```  
  
## <a name="arguments"></a>Arguments  
 CRÉER la TABLE &#124; DBF *TableName1*  
 Spécifie le nom de la table à créer. Les options de TABLE et DBF sont identiques.  
  
 NOM *LongTableName*  
 Spécifie un nom long de la table. Un nom de table long peut être spécifié uniquement quand une base de données est ouvert, car les noms de table longs sont stockées dans les bases de données.  
  
 Les noms longs peuvent contenir jusqu'à 128 caractères et peuvent être utilisés à la place des noms de fichiers courts dans la base de données.  
  
 GRATUIT  
 Spécifie que la table n’est pas ajoutée à une base de données ouverte. LIBRE n’est pas nécessaire si une base de données n’est pas ouverte.  
  
 *(Nom_champ1 FieldType* [( *nFieldWidth* [, *nPrecision*])]  
 Spécifie le nom du champ, type de champ, largeur de champ et de champ précision (nombre de décimales), respectivement.  
  
 *FieldType* est une lettre unique indiquant du champ [type de données](../../odbc/microsoft/visual-foxpro-field-data-types.md). Certains types de données de champ requièrent que vous spécifiez *nFieldWidth* ou *nPrecision* ou les deux.  
  
 *nFieldWidth* et *nPrecision* sont ignorés pour D, G, I, L, M, P, T et Y types. *nPrecision* par défaut est zéro (pas de décimales) si *nPrecision* n’est pas incluse pour les types B, F ou N.  
  
 NULL  
 Autorise les valeurs null dans le champ.  
  
 NOT NULL  
 Empêche des valeurs null dans le champ.  
  
 Si vous omettez la valeur NULL et NOT NULL, la valeur actuelle de SET NULL détermine si les valeurs null sont autorisées dans le champ. Toutefois, si vous omettez la valeur NULL et non NULL et incluez la clé primaire ou une clause UNIQUE, le paramètre actuel de la valeur NULL est ignoré et le champ de valeur par défaut est NOT NULL.  
  
 Vérifiez *lExpression1*  
 Spécifie une règle de validation pour le champ. *lExpression1* peut être une fonction définie par l’utilisateur. Chaque fois qu’un enregistrement vide est ajouté, la règle de validation est activée. Une erreur est générée si la règle de validation n’autorise pas une valeur de champ dans un enregistrement ajouté.  
  
 ERREUR *cMessageText1*  
 Spécifie le message d’erreur que Visual FoxPro s’affiche lorsque la règle de champ génère une erreur. Le message s’affiche uniquement lorsque les données sont modifiées dans une fenêtre de navigation ou d’une fenêtre d’édition.  
  
 Par défaut *eExpression1*  
 Spécifie une valeur par défaut pour le champ. Type de données de *eExpression1* doit être le même que le type de données.  
  
 PRIMARY KEY  
 Crée un index principal pour le champ. La balise de l’index primaire a le même nom que le champ.  
  
 UNIQUE  
 Crée un index candidats pour le champ. La balise d’index candidat a le même nom que le champ.  
  
> [!NOTE]  
>  Candidat les index (en incluant l’option UNIQUE dans CREATE TABLE ou ALTER TABLE - SQL) ne sont pas le même que les index créés avec l’option UNIQUE dans la commande de l’INDEX. Un index créé avec l’option UNIQUE dans la commande de l’INDEX autorise les clés d’index en double ; index candidats n’autorisent pas les clés d’index en double. Consultez [INDEX](../../odbc/microsoft/index-command.md) pour plus d’informations sur son option UNIQUE.  
  
 Les valeurs NULL et les enregistrements en double ne sont pas autorisés dans un champ utilisé pour un index primaire ou candidate. Toutefois, Visual FoxPro ne génère pas d’une erreur si vous créez un index primaire ou candidate pour un champ qui prend en charge les valeurs null. Visual FoxPro génère une erreur si vous tentez d’entrer une valeur null ou en double dans un champ utilisé pour un index primaire ou candidate.  
  
 RÉFÉRENCES *TableName2*[balise *TagName1*]  
 Spécifie la table parente à laquelle une relation permanente est établie. Si vous omettez la balise *TagName1*, la relation est établie à l’aide de la clé d’index primaire de la table parente. Si la table parente n’a pas d’un index principal, Visual FoxPro génère une erreur.  
  
 Include (balise) *TagName1* pour établir une relation basée sur une balise d’index existante pour la table parente. Noms de balise d’index peuvent contenir jusqu'à 10 caractères.  
  
 La table parente ne peut pas être une table indépendante.  
  
 NOCPTRANS  
 Empêche la traduction à une autre page de codes pour les champs de type caractère et Mémo. Si la table est convertie en une autre page de codes, les champs pour lesquels NOCPTRANS a été spécifié ne sont pas traduits. NOCPTRANS peut être spécifié uniquement pour les champs de type caractère et Mémo.  
  
 L’exemple suivant crée une table intitulée mytable contenant deux champs de type caractère et les deux champs de type memo. Le deuxième champ de caractère et le deuxième champ de type memo, memo2, char2 incluent NOCPTRANS pour empêcher la traduction.  
  
```  
CREATE TABLE mytable (char1 C(10), char2 C(10) NOCPTRANS,;  
   memo1 M, memo2 M NOCPTRANS)  
```  
  
 CLÉ primaire *eExpression2* balise *TagName2*  
 Spécifie un index primaire à créer. *eExpression2* spécifie un champ ou une combinaison de champs dans la table. BALISE *TagName2 s*Spécifie le nom de la balise de l’index primaire qui est créé. Noms de balise d’index peuvent contenir jusqu'à 10 caractères.  
  
 Comme une table peut avoir qu’un seul index principal, vous ne pouvez inclure cette clause si vous avez déjà créé un index primaire pour un champ. Visual FoxPro génère une erreur si vous incluez plusieurs clauses de clé primaire dans CREATE TABLE.  
  
 UNIQUE *eExpression3*balise *TagName3*  
 Crée un index candidats. *eExpression3* spécifie un champ ou une combinaison de champs dans la table. Toutefois, si vous avez créé un index principal avec une des options de la clé primaire, vous ne pouvez inclure le champ qui a été spécifié pour l’index primaire. BALISE *TagName3 s*spécifie un nom de balise pour la balise index candidats qui est créé. Noms de balise d’index peuvent contenir jusqu'à 10 caractères.  
  
 Une table peut avoir plusieurs index candidats.  
  
 CLÉ étrangère *eExpression4*balise *TagName4*[NODUP]  
 Crée un index (non essentielles) étrangère et établit une relation à une table parente. *eExpression4* Spécifie l’expression de clé étrangère d’index, et *TagName4* Spécifie le nom de la balise de clé étrangère index créé *.* Noms de balise d’index peuvent contenir jusqu'à 10 caractères. Inclure NODUP pour créer un index étrangère candidat.  
  
 Vous pouvez créer plusieurs index étrangères pour la table, mais les expressions d’index étrangère doivent spécifier les différents champs de la table.  
  
 RÉFÉRENCES *TableName3*[balise *TagName5*]  
 Spécifie la table parente à laquelle une relation permanente est établie. Include (balise) *TagName5* pour établir une relation basée sur une balise d’index pour la table parente. Noms de balise d’index peuvent contenir jusqu'à 10 caractères. Par défaut, si vous omettez la balise *TagName5,* la relation est établie à l’aide de la clé d’index primaire de la table parente.  
  
 Vérifiez *eExpression2*[erreur *cMessageText2*]  
 Spécifie la règle de validation de table. ERREUR *cMessageText2* Spécifie le message d’erreur Visual FoxPro affiche lors de l’exécution de la règle de validation de table. Le message s’affiche uniquement lorsque des données sont modifiées au sein d’une fenêtre de navigation ou modifier la fenêtre.  
  
 À partir du tableau *nom de tableau*  
 Spécifie le nom d’un tableau existant dont le contenu est le nom, type, la précision et l’échelle pour chaque champ dans la table. Le contenu du tableau peut être défini avec la **AFIELDS**(fonction) ().  
  
## <a name="remarks"></a>Notes  
 La nouvelle table est ouvert dans la zone la plus basse de travail disponible et est accessible par son alias. La nouvelle table est ouverte en mode exclusif, quel que soit le paramètre actuel de la valeur exclusif.  
  
 Si une base de données est ouverte et que vous n’incluez pas la clause libre, la nouvelle table est ajoutée à la base de données. Impossible de créer une nouvelle table avec le même nom qu’une table dans la base de données.  
  
 Si une base de données est ouverte, CREATE TABLE - SQL requiert une utilisation exclusive de la base de données. Pour ouvrir une base de données pour une utilisation exclusive, inclure exclusif d’ouvrir la base de données.  
  
 Si une base de données n’est pas ouverte lorsque vous créez la nouvelle table, y compris les clauses de nom, vérification, par défaut, FOREIGN KEY, PRIMARY KEY ou références génère une erreur.  
  
> [!NOTE]  
>  Syntaxe CREATE TABLE utilise des virgules pour séparer certaines options CREATE TABLE. En outre, NULL, pas NULL, chèque, par défaut, clé primaire et une clause UNIQUE doivent être placés entre parenthèses contenant les définitions de colonne.  
  
## <a name="driver-remarks"></a>Section Notes de pilote  
 Lorsque votre application envoie l’instruction ODBC SQL CREATE TABLE pour la source de données, le pilote ODBC Visual FoxPro traduit la commande dans la ligne de commande Visual FoxProCREATE TABLE à l’aide de la syntaxe indiquée dans le tableau suivant.  
  
|Syntaxe ODBC|Syntaxe de Visual FoxPro|  
|-----------------|--------------------------|  
|CREATE TABLE *nom de table de base*<br /><br /> (*type de données d’identificateur de la colonne*<br /><br /> [NON NULL]<br /><br /> [,*type de données d’identificateur de la colonne*<br /><br /> [NON NULL]...)|CRÉER la TABLE *TableName1* [nom *LongTableName*]<br /><br /> (*Nom_champ1* *FieldType*<br /><br /> [(*nFieldWidth* [, *nPrecision*])]<br /><br /> [NON NULL)]|  
  
 Lorsque vous créez une table à l’aide du pilote, le pilote ferme la table immédiatement après la création d’autoriser l’accès à la table par d’autres utilisateurs. Cela diffère de Visual FoxPro, ce qui laisse l’ouvert exclusivement lors de la création de la table. Toutefois, si l’exécution d’une procédure stockée sur votre source de données qui contient une instruction CREATE TABLE, la table est laissée ouverte.  
  
 Si la source de données est une base de données (fichier .dbc), le pilote ODBC Visual FoxPro crée une table nommée *LongTableName* avec le même nom que le *nom de table de base de*.  
  
### <a name="using-data-definition-language-ddl"></a>À l’aide du langage de définition de données (DDL)  
 Vous ne pouvez inclure DDL dans les emplacements suivants :  
  
-   Dans une instruction SQL par lots qui requiert une transaction  
  
-   En mode de validation manuelle, après une instruction qui requiert une transaction, sauf si votre application appelle d’abord **SQLTransact**.  
  
 Par exemple, si vous souhaitez créer une table temporaire, vous devez créer la table avant de commencer l’instruction qui requiert une transaction. Si vous incluez l’instruction CREATE TABLE dans une instruction SQL qui requiert une transaction par lots, le pilote retourne un message d’erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE - commande SQL](../../odbc/microsoft/alter-table-sql-command.md)   
 [Types de données pris en charge (le pilote ODBC Visual FoxPro)](../../odbc/microsoft/supported-data-types-visual-foxpro-odbc-driver.md)   
 [INSERT - commande SQL](../../odbc/microsoft/insert-sql-command.md)   
 [SELECT, commande SQL](../../odbc/microsoft/select-sql-command.md)
