---
title: Charger les données avec INSERT - Parallel Data Warehouse | Documents Microsoft
description: Pour charger des données dans un Parallel Data Warehouse (PDW) à l’aide de l’instruction T-SQL INSERT distribuées ou table répliquée.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: b7a05e5381c2ad687c37926ad449cd6765403ceb
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34585781"
---
# <a name="load-data-with-insert-into-parallel-data-warehouse"></a>Charger des données avec INSERT dans Parallel Data Warehouse

Vous pouvez utiliser l’instruction INSERT tsql pour charger des données dans un SQL Server Parallel Data Warehouse (PDW) distribuées ou table répliquée. Pour plus d’informations sur l’insertion, consultez [insérer](../t-sql/statements/insert-transact-sql.md). Pour les tables répliquées et toutes les colonnes non-distribution dans une table distribuée, PDW utilise SQL Server pour convertir implicitement les valeurs de données spécifiées dans l’instruction pour le type de données de la colonne de destination. Pour plus d’informations sur les règles de conversion de données SQL Server, consultez [de types de données pour SQL](http://msdn.microsoft.com/library/ms191530\(v=sql11\).aspx). Toutefois, pour les colonnes de distribution, PDW prend en charge uniquement un sous-ensemble des conversions implicites qui prend en charge de SQL Server. Par conséquent, lorsque vous utilisez l’instruction INSERT pour charger des données dans une colonne de distribution, la source de données doit être spécifié dans un des formats définis dans les tableaux suivants.  
  
  
## <a name="InsertingLiteralsBinary"></a>Insérer des littéraux dans les types binaires  
Le tableau suivant définit les types de littéral acceptés, format et les règles de conversion pour l’insertion d’une valeur littérale dans une colonne de distribution de type **binaire** (*n*) ou **varbinary** (*n*).  
  
|Type de littéral|Format|Règles de conversion|  
|----------------|----------|--------------------|  
|Littéral binaire|0x*hexidecimal_string*<br /><br />Exemple : 0x12Ef|Les littéraux binaires doivent être précédés de 0 x.<br /><br />La longueur de source de données ne peut pas dépasser le nombre d’octets spécifié pour le type de données.<br /><br />Si la longueur de source de données est inférieure à la taille de la **binaire** de type de données, les données sont complétées à droite avec des zéros non significatifs pour atteindre la taille de type de données.|  
  
## <a name="InsertingLiteralsDateTime"></a>Insérer des littéraux dans les types de date et d’heure  
Les littéraux de date et d’heure sont représentées à l’aide des valeurs de caractère dans un format spécifique, placé entre guillemets simples. Les tableaux suivants décrivent les types de littéral autorisés, le format et les règles de conversion pour l’insertion d’une date ou un littéral d’heure dans une colonne de distribution SQL Server PDW de type **datetime**, **smalldatetime**, **date**, **temps**, **datetimeoffset**, ou **datetime2**.  
  
### <a name="datetime-data-type"></a>type de données DateTime  
Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **datetime**. Toute chaîne vide (") est converti en la valeur par défaut ' 1900-01-01 12:00:00.000'. Les chaînes qui contiennent uniquement des espaces à droite (' ') génère une erreur.  
  
|Type de littéral|Format|Règles de conversion|  
|----------------|----------|--------------------|  
|Littéral de chaîne dans **datetime** format|« AAAA-MM-JJ HH : mm : [.nnn] »<br /><br />Exemple : « 2007-05-08 12:35:29.123'|Les chiffres fractionnaires manquantes sont définies à 0 lorsque la valeur est insérée. Par exemple, le littéral ' 2007-05-08 12:35 ' est inséré en tant que « 2007-05-08 12:35:00.000'.|  
|Littéral de chaîne dans **smalldatetime** format|'AAAA-MM-JJ HH'<br /><br />Exemple : « 2007-05-08 12:35 '|Secondes et fractions restantes sont définies à 0 lorsque la valeur est insérée.|  
|Littéral de chaîne dans **date** format|« AAAA-MM-JJ »<br /><br />Exemple : « 2007-05-08'|Les valeurs de temps (heures, minutes, secondes et fractions) sont définies sur 12:00:00.000 lorsque la valeur est insérée.|  
|Littéral de chaîne dans **datetime2** format|'AAAA-MM-JJ.nnnnnnn'<br /><br />Exemple : « 2007-05-08 12:35:29.1234567'|La source de données ne peut pas dépasser trois chiffres fractionnaires. Par exemple, le littéral ' 2007-05-08 12:35:29.123' sera inséré, mais la valeur « 2007-05-08 12:35:29.1234567' génère une erreur.|  
  
### <a name="smalldatetime-data-type"></a>type de données smalldatetime  
Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **smalldatetime**. Toute chaîne vide (") est converti en la valeur par défaut ' 01 / 01/1900 12:00 ». Les chaînes qui contiennent uniquement des espaces à droite (' ') génère une erreur.  
  
|Type de littéral|Format|Règles de conversion|  
|----------------|----------|--------------------|  
|Littéral de chaîne dans **smalldatetime** format|'AAAA-MM-JJ HH : mm' ou 'AAAA-MM-JJ hh:mm:00'<br /><br />Exemple : « 2007-05-08 12:00 ' ou ' 2007-05-08-12:00:00 '|La source de données doit avoir des valeurs pour l’année, mois, date, heure et minute. Secondes sont facultatives et, s’il est présent, doivent être définies à la valeur 00. Toute autre valeur génère une erreur.|  
|Littéral de chaîne dans **date** format|« AAAA-MM-JJ »<br /><br />Exemple : « 2007-05-08'|Valeurs d’heure (heures, minutes, secondes et fractions) sont définies à 0 lorsque la valeur est insérée.|  
  
### <a name="date-data-type"></a>type de données date  
Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **date**. Toute chaîne vide (") est converti en la valeur par défaut ' 1900-01-01 ». Les chaînes qui contiennent uniquement des espaces à droite (' ') génère une erreur.  
  
|Type de littéral|Format|Règles de conversion|  
|----------------|----------|--------------------|  
|Littéral de chaîne dans **date** format|« AAAA-MM-JJ »<br /><br />Exemple : « 2007-05-08'|Il s’agit du seul format accepté.|  
  
### <a name="time-data-type"></a>Type de données d’heure  
Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **temps**. Toute chaîne vide (") est convertie en la valeur par défaut '00:00:00.0000'. Les chaînes qui contiennent uniquement des espaces à droite (' ') génère une erreur.  
  
|Type de littéral|Format|Règles de conversion|  
|----------------|----------|--------------------|  
|Littéral de chaîne dans **temps** format|'.nnnnnnn'<br /><br />Exemple : '12:35:29.1234567'|Si la source de données a une précision inférieure ou égale (nombre de chiffres fractionnaires) à la précision de la **temps** type de données, les données sont complétées à droite avec des zéros. Par exemple, une valeur littérale '12:35:29.123' est insérée en tant que '12:35:29.1230000'.<br /><br />Une valeur qui a une précision plus élevée que le type de données cible est rejetée.|  
  
### <a name="datetimeoffset-data-type"></a>type de données DateTimeOffset  
Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **datetimeoffset** (*n*). Le format par défaut est ' AAAA-MM-JJ.nnnnnnn {+ |-} hh : mm ». Une chaîne vide (") est convertie en la valeur par défaut ' 1900-01-01 12:00:00.0000000 + 00:00 ». Les chaînes qui contiennent uniquement des espaces à droite (' ') génère une erreur. Le nombre de chiffres fractionnaires dépend de la définition de colonne. Par exemple, une colonne définie en tant que **datetimeoffset** (2) aura deux chiffres fractionnaires.  
  
|Type de littéral|Format|Règles de conversion|  
|----------------|----------|--------------------|  
|Littéral de chaîne dans **datetime** format|« AAAA-MM-JJ HH : mm : [.nnn] »<br /><br />Exemple : « 2007-05-08 12:35:29.123'|Les chiffres fractionnaires manquantes et les valeurs de décalage sont définies à 0 lorsque la valeur est insérée. Par exemple, le littéral ' 2007-05-08 12:35:29.123 » est inséré en tant que « 2007-05-08 12:35:29.1230000 + 00:00 ».|  
|Littéral de chaîne dans **smalldatetime** format|'AAAA-MM-JJ HH'<br /><br />Exemple : « 2007-05-08 12:35 '|Secondes, les chiffres fractionnaires restants et les valeurs de décalage sont définies à 0 lorsque la valeur est insérée.|  
|Littéral de chaîne dans **date** format|« AAAA-MM-JJ »<br /><br />Exemple : « 2007-05-08'|Valeurs d’heure (heures, minutes, secondes et fractions) sont définies à 0 lorsque la valeur est insérée. Par exemple, le littéral ' 2007-05-08' est inséré en tant que « 2007-05-08 00:00:00.0000000 + 00:00 ».|  
|Littéral de chaîne dans **datetime2** format|'AAAA-MM-JJ.nnnnnnn'<br /><br />Exemple : « 2007-05-08 12:35:29.1234567'|La source de données ne peut pas dépasser le nombre spécifié de fraction de seconde dans la colonne de datetimeoffset. Si la source de données possède un nombre plus petit ou égal de fraction de seconde, les données sont complétées à droite avec des zéros. Par exemple, si le type de données est datetimeoffset (5), la valeur littérale ' 2007-05-08 12:35:29.123 + 12:15 ' est inséré en tant que ' 12:35:29.12300 + 12:15 '.|  
|Littéral de chaîne dans **datetimeoffset** format|' AAAA-MM-JJ.nnnnnnn {+&#124;-} hh : mm '<br /><br />Exemple : « 2007-05-08 12:35:29.1234567 + 12:15 '|La source de données ne peut pas dépasser le nombre spécifié de fraction de seconde dans la colonne de datetimeoffset. Si la source de données possède un nombre plus petit ou égal de fraction de seconde, les données sont complétées à droite avec des zéros. Par exemple, si le type de données est datetimeoffset (5), la valeur littérale ' 2007-05-08 12:35:29.123 + 12:15 ' est inséré en tant que ' 12:35:29.12300 + 12:15 '.|  
  
### <a name="datetime2-data-type"></a>type de données DATETIME2  
Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **datetime2** (*n*). Le format par défaut est 'AAAA-MM-JJ.nnnnnnn'. Une chaîne vide (") est convertie en la valeur par défaut ' 1900-01-01-12:00:00 ». Les chaînes qui contiennent uniquement des espaces à droite (' ') génère une erreur. Le nombre de chiffres fractionnaires dépend de la définition de colonne. Par exemple, une colonne définie en tant que **datetime2** (2) aura deux chiffres fractionnaires.  
  
|Type de littéral|Format|Règles de conversion|  
|----------------|----------|--------------------|  
|Littéral de chaîne dans **datetime** format|« AAAA-MM-JJ HH : mm : [.nnn] »<br /><br />Exemple : « 2007-05-08 12:35:29.123'|Les fractions de secondes sont facultatives et sont définies sur 0 lorsque la valeur est insérée.<br /><br />Une valeur qui compte davantage de chiffres fractionnaires que le type de données cible est rejetée.|  
|Littéral de chaîne dans **smalldatetime** format|'AAAA-MM-JJ HH'<br /><br />Exemple : « 2007-05-08 12'|Secondes facultatif et chiffres fractionnaires restants sont égales à 0 lorsque la valeur est insérée.|  
|Littéral de chaîne dans **date** format|« AAAA-MM-JJ »<br /><br />Exemple : « 2007-05-08'|Valeurs d’heure (heures, minutes, secondes et fractions) sont définies à 0 lorsque la valeur est insérée. Par exemple, le littéral ' 2007-05-08' est inséré en tant que « 2007-05-08 12:00:00.0000000'.|  
|Littéral de chaîne dans **datetime2** format|'AAAA-MM-JJ hh:mm:ss:nnnnnnn'<br /><br />Exemple : « 2007-05-08 12:35:29.1234567'|Si la source de données contient des composants de date et d’heure qui sont inférieur ou égal à la valeur spécifiée dans **datetime2**(*n*), les données sont insérées ; sinon, une erreur est générée.|  
  
## <a name="InsertLiteralsNumeric"></a>Insérer des littéraux dans les types numériques  
Les tableaux suivants décrivent les formats acceptés et les règles de conversion pour l’insertion d’une valeur littérale dans une colonne de distribution de SQL Server PDW qui utilise un type numérique.  
  
### <a name="bit-data-type"></a>type de données bit  
Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **bits**. Une chaîne vide (") ou une chaîne qui contiennent uniquement des espaces à droite (' ') est convertie en 0.  
  
|Type de littéral|format|Règles de conversion|  
|----------------|----------|--------------------|  
|Littéral de chaîne dans **entier** format|'nnnnnnnnnn'<br /><br />Exemple : '1' ou '321'|Une valeur entière mise en forme comme un littéral de chaîne ne peut pas contenir une valeur négative. Par exemple, la valeur '-' 123 génère une erreur.<br /><br />Une valeur supérieure à 1 est convertie en 1. Par exemple, la valeur « 123 » est convertie 1.|  
|Littéral de chaîne|'TRUE' ou 'FALSE'<br /><br />Exemple : 'true'|La valeur 'TRUE' est converti en 1 ; la valeur 'FALSE' est convertie en 0.|  
|Littéral d’entier|nnnnnnnn<br /><br />Exemple : 1 ou 321|Une valeur supérieure à 1 ou inférieure à 0 est convertie en 1. Par exemple, les valeurs 123 et -123 sont converties à 1.|  
|Littéral décimal|nnnnn.nnnn<br /><br />Exemple : 1234.5678|Une valeur supérieure à 1 ou inférieure à 0 est convertie en 1. Par exemple, les valeurs 123,45 et -123,45 sont converties à 1.|  
  
### <a name="decimal-data-type"></a>type de données decimal  
Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **décimal** (*p, s*). Les règles de conversion de données sont les mêmes que pour SQL Server. Pour plus d’informations, consultez [conversion de type de données](http://msdn.microsoft.com/library/ms191530&#40;v=sql11&#40;.aspx) sur MSDN.  
  
|Type de littéral|Format|  
|----------------|----------|  
|Littéral de chaîne dans **entier** format|'nnnnnnnnnnnn'<br /><br />Exemple : '321312313123'|  
|Littéral de chaîne dans **décimal** format|'nnnnnn.nnnnn'<br /><br />Exemple : '123344.34455'|  
|Littéral d’entier|nnnnnnnnnnnn<br /><br />Exemple : 321312313123|  
|Littéral décimal|nnnnnn.nnnnn<br /><br />Exemple : '123344.34455'|  
  
### <a name="float-and-real-data-types"></a>types de données float et real  
Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **float** ou **réel**. Les règles de conversion de données sont les mêmes que pour SQL Server. Pour plus d’informations, consultez [conversion de type de données](http://msdn.microsoft.com/library/ms191530&#40;v=sql11&#40;.aspx) sur MSDN.  
  
|Type de littéral|Format|  
|----------------|----------|  
|Littéral de chaîne dans **entier** format|'nnnnnnnnnnnn'<br /><br />Exemple : '321312313123'|  
|Littéral de chaîne dans **décimal** format|'nnnnnn.nnnnn'<br /><br />Exemple : '123344.34455'|  
|Littéral de chaîne dans **à virgule flottante** format|'n.nnnnnE+nn'<br /><br />Exemple : « 3.12323E + 14 »|  
|Littéral d’entier|nnnnnnnnnnnn<br /><br />Exemple : 321312313123|  
|Littéral décimal|nnnnnn.nnnnn<br /><br />Exemple : 123344.34455|  
|Littéral de point flottant|n.nnnnnE+nn<br /><br />Exemple : 3.12323E + 14|  
  
### <a name="int-bigint-tinyint-smallint-data-types"></a>int, bigint, tinyint, smallint des types de données  
Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **int**, **bigint**, **tinyint**, ou **smallint**. La source de données ne peut pas dépasser la plage autorisée pour le type de données spécifiée. Par exemple, la plage de **tinyint** est comprise entre 0 et 255 et la plage pour **int** est allant de -2,147,483,648 à 2 147 483 647.  
  
|Type littéral|Format|Règles de conversion|  
|------------|------|----------------|
|Littéral de chaîne dans **entier** format|'nnnnnnnnnnnnnn'<br /><br />Exemple : '321312313123'| None |  
|Littéral d’entier|nnnnnnnnnnnnnn<br /><br />Exemple : 321312313123| None|  
|Littéral décimal|nnnnnn.nnnnn<br /><br />Exemple : 123344.34455|Les valeurs à droite du séparateur décimal sont tronquées.|  
  
### <a name="money-and-smallmoney-data-types"></a>types de données Money et smallmoney  
Les valeurs littérales Money sont représentées sous forme de nombres avec un séparateur décimal facultatif et le symbole monétaire en tant que préfixe. La source de données ne peut pas dépasser la plage autorisée pour le type de données spécifiée. Par exemple, la plage de **smallmoney** est entre-214 748,3648 et 214 748,3647 la plage pour **money** est -922,337,203,685,477.5808 à 922,337,203,685,477.5807. Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **money** ou **smallmoney**.  
  
|Type de littéral|Format|Règles de conversion|  
|----------------|----------|--------------------|  
|Littéral de chaîne dans **entier** format|'nnnnnnnn'<br /><br />Exemple : '123433'|Chiffres manquants après la virgule décimale sont définies à 0 lorsque la valeur est insérée. Par exemple, le littéral « 12345 » est inséré comme 12345.0000.|  
|Littéral de chaîne dans **décimal** format|'nnnnnn.nnnnn'<br /><br />Exemple : '123344.34455'|Si le nombre de chiffres après la virgule décimale dépasse 4, la valeur est arrondie à la valeur la plus proche. Par exemple, la valeur « 123344.34455 » est insérée en tant que 123344.3446.|  
|Littéral de chaîne dans **money** format|'$nnnnnn.nnnn'<br /><br />Exemple : '$123456.7890'|Le symbole monétaire n’est pas inséré avec la valeur.<br /><br />Si le nombre de chiffres après la virgule décimale dépasse 4, la valeur est arrondie à la valeur la plus proche.|  
|Littéral d’entier|nnnnnnnn<br /><br />Exemple : 123433|Chiffres manquants après la virgule décimale sont définies à 0 lorsque la valeur est insérée. Par exemple, le littéral 12345 est inséré comme 12345.0000.|  
|Littéral décimal|nnnnnn.nnnnn<br /><br />Exemple : 123344.34455|Si le nombre de chiffres après la virgule décimale dépasse 4, la valeur est arrondie à la valeur la plus proche. Par exemple, la valeur 123344.34455 est insérée en tant que 123344.3446.|  
|Money littéral|$nnnnnn.nnnn<br /><br />Exemple : $123456.7890|Le symbole monétaire n’est pas inséré avec la valeur.<br /><br />Si le nombre de chiffres après la virgule décimale dépasse 4, la valeur est arrondie à la valeur la plus proche.|  
  
## <a name="InsertLiteralsString"></a>Insertion de littéraux dans les types de chaînes  
Les tableaux suivants décrivent les formats acceptés et les règles de conversion pour l’insertion d’une valeur littérale dans une colonne SQL Server PDW qui utilise un type chaîne.  
  
### <a name="char-varchar-nchar-and-nvarchar-data-types"></a>types de données char, varchar, nchar et nvarchar  
Le tableau suivant définit les formats acceptés et les règles pour insérer des valeurs littérales dans une colonne de distribution de type **char**, **varchar**, **nchar** et **nvarchar**. La longueur de source de données ne peut pas dépasser la taille spécifiée pour le type de données. Si la longueur de source de données est inférieure à la taille de la **char** ou **nchar** type de données, les données sont complétées à droite avec des espaces vides pour atteindre la taille de type de données.  
  
|Type littéral|Format|Règles de conversion|  
|----------------|----------|--------------------|  
|Littéral de chaîne|Format : « chaîne de caractères'<br /><br />Exemple : 'abc'| None|  
|Littéral de chaîne Unicode|Format : Chaîne N'character'<br /><br />Exemple : N’ABC »|  None |  
|Littéral d’entier|Format : nnnnnnnnnnn<br /><br />Exemple : 321312313123| None |  
|Littéral décimal|Format : nnnnnn.nnnnnnn<br /><br />Exemple : 12344.34455| None |  
|Money littéral|Format : $nnnnnn.nnnnn<br /><br />Exemple : $123456.99|Le symbole de devise n’est pas inséré avec la valeur. Pour insérer le symbole de devise, insérez la valeur comme un littéral de chaîne. Cela fera correspondre le format de la **dwloader** outil qui traite chaque littéral comme un littéral de chaîne.<br /><br />Des virgules ne sont pas autorisés.<br /><br />Si le nombre de chiffres après la virgule décimale dépasse 2, la valeur est arrondie à la valeur la plus proche. Par exemple, la valeur 123.946789 est insérée en tant que 123.95.<br /><br />Seul le style par défaut 0 (pas de virgule et 2 chiffres après la virgule décimale) est autorisé lors de l’utilisation de la fonction CONVERT pour insérer des littéraux de l’argent.|  

  
## <a name="see-also"></a>Voir aussi  
 
[Données distribuées](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-distributed-data/)  
[INSERT](../t-sql/statements/insert-transact-sql.md)  
  
<!-- MISSING LINKS
[Grant permissions to load data](grant-permissions-to-load-data.md)  
[Metadata query examples](metadata-query-examples.md) 
-->
