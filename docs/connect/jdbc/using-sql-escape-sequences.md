---
title: À l’aide de séquences d’échappement SQL | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 00f9e25a-088e-4ac6-aa75-43eacace8f03
caps.latest.revision: 25
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 60af5198eb78ad23bf8ad55a1dc24690b91130ab
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-sql-escape-sequences"></a>Utilisation de séquences d'échappement SQL
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] prend en charge l’utilisation des séquences d’échappement SQL, tel que défini par l’API JDBC. Les séquences d'échappement sont utilisées dans une instruction SQL pour indiquer au pilote que la partie d'échappement de la chaîne SQL doit être gérée différemment. Lorsque le pilote JDBC traite la partie d'échappement d'une chaîne SQL, il traduit cette partie de la chaîne en code SQL compris par SQL Server.  
  
 Il existe cinq types de séquences d'échappement requis par l'API JDBC, et tous sont pris en charge par le pilote JDBC :  
  
-   Littéraux génériques LIKE  
  
-   Gestion de fonction  
  
-   Littéraux de date et heure  
  
-   Appels de procédures stockées  
  
-   Jointures externes  
  
-   Syntaxe d'échappement LIMIT  
  
 La syntaxe de séquence d'échappement utilisée par le pilote JDBC est la suivante :  
  
 `{keyword ...parameters...}`  
  
> [!NOTE]  
>  Le traitement d'échappement SQL est toujours activé pour le pilote JDBC.  
  
 Les sections suivantes décrivent les cinq types de séquences d'échappement et comment ils sont pris en charge par le pilote JDBC.  
  
## <a name="like-wildcard-literals"></a>Littéraux génériques LIKE  
 Le pilote JDBC prend en charge la `{escape 'escape character'}` syntaxe pour l’utilisation de caractères génériques de clause LIKE comme littéraux. Par exemple, le code suivant retournera des valeurs pour col3, où la valeur de col2 commence littéralement par un trait de soulignement (et pas son utilisation générique).  
  
```  
ResultSet rst = stmt.executeQuery("SELECT col3 FROM test1 WHERE col2   
LIKE '\\_%' {escape '\\'}");  
```  
  
> [!NOTE]  
>  La séquence d'échappement doit être à la fin de l'instruction SQL. Pour plusieurs instructions SQL dans une chaîne de commande, la séquence d'échappement doit être à la fin de chaque instruction SQL pertinente.  
  
## <a name="function-handling"></a>Gestion de fonction  
 Le pilote JDBC prend en charge les séquences d'échappement de fonction dans les instructions SQL avec la syntaxe suivante :  
  
```  
{fn functionName}  
```  
  
 où `functionName` est une fonction prise en charge par le pilote JDBC. Par exemple :  
  
```  
SELECT {fn UCASE(Name)} FROM Employee  
```  
  
 Le tableau suivant répertorie les différentes fonctions prises en charge par le pilote JDBC lors de l'utilisation d'une séquence d'échappement de fonction :  
  
|Fonctions de chaîne|Fonctions numériques|Fonctions datetime|Fonctions système|  
|----------------------|-----------------------|------------------------|----------------------|  
|ASCII<br /><br /> CHAR<br /><br /> CONCAT<br /><br /> DIFFERENCE<br /><br /> INSERT<br /><br /> LCASE<br /><br /> LEFT<br /><br /> LENGTH<br /><br /> LOCATE<br /><br /> LTRIM<br /><br /> REPEAT<br /><br /> REPLACE<br /><br /> RIGHT<br /><br /> RTRIM<br /><br /> SOUNDEX<br /><br /> SPACE<br /><br /> SUBSTRING<br /><br /> UCASE|ABS<br /><br /> ACOS<br /><br /> ASIN<br /><br /> ATAN<br /><br /> ATAN2<br /><br /> CEILING<br /><br /> COS<br /><br /> COT<br /><br /> DEGREES<br /><br /> EXP<br /><br /> FLOOR<br /><br /> LOG<br /><br /> LOG10<br /><br /> MOD<br /><br /> PI<br /><br /> POWER<br /><br /> RADIANS<br /><br /> RAND<br /><br /> ROUND<br /><br /> SIGN<br /><br /> SIN<br /><br /> SQRT<br /><br /> TAN<br /><br /> TRUNCATE|CURDATE<br /><br /> CURTIME<br /><br /> DAYNAME<br /><br /> DAYOFMONTH<br /><br /> DAYOFWEEK<br /><br /> DAYOFYEAR<br /><br /> EXTRACT<br /><br /> HOUR<br /><br /> MINUTE<br /><br /> MONTH<br /><br /> MONTHNAME<br /><br /> NOW<br /><br /> QUARTER<br /><br /> SECOND<br /><br /> TIMESTAMPADD<br /><br /> TIMESTAMPDIFF<br /><br /> WEEK<br /><br /> YEAR|DATABASE<br /><br /> IFNULL<br /><br /> Utilisateur|  
  
> [!NOTE]  
>  Si vous essayez d'utiliser une fonction que la base de données ne prend pas en charge, une erreur se produit.  
  
## <a name="date-and-time-literals"></a>Date et heure  
 La syntaxe d'échappement pour les littéraux de date, heure et horodatage est la suivante :  
  
```  
{literal-type 'value'}  
```  
  
 où `literal-type` est une des opérations suivantes :  
  
|Type littéral| Description|Format de la valeur|  
|------------------|-----------------|------------------|  
|d|Date|aaaa-mm-jj|  
|t|Time|hh:mm:ss [1]|  
|ts|TimeStamp|aaaa-mm-jj hh:mm:ss[.f...]|  
  
 Par exemple :  
  
```  
UPDATE Orders SET OpenDate={d '2005-01-31'}   
WHERE OrderID=1025  
```  
  
## <a name="stored-procedure-calls"></a>Appels de procédure stockée  
 Le pilote JDBC prend en charge la `{? = call proc_name(?,...)}` et `{call proc_name(?,...)}` d’échappement de syntaxe pour les appels de procédure stockée, selon que vous devez traiter un paramètre de retour.  
  
 Une procédure est un objet exécutable stocké dans la base de données. Généralement, il s'agit d'une ou plusieurs instructions SQL qui ont été précompilées. La syntaxe de séquence d'échappement pour appeler une procédure stockée est la suivante :  
  
```  
{[?=]call procedure-name[([parameter][,[parameter]]...)]}  
```  
  
 où `procedure-name` Spécifie le nom d’une procédure stockée et `parameter` spécifie un paramètre de procédure stockée.  
  
 Pour plus d’informations sur l’utilisation de la `call` échappement avec des procédures stockées, consultez [à l’aide d’instructions avec des procédures stockées](../../connect/jdbc/using-statements-with-stored-procedures.md).  
  
## <a name="outer-joins"></a>Jointures externes  
 Le pilote JDBC prend en charge la syntaxe de jointure externe SQL92 gauche, droite et entière. La séquence d'échappement pour les jointures externes est la suivante :  
  
```  
{oj outer-join}  
```  
  
 où outer-join est :  
  
```  
table-reference {LEFT | RIGHT | FULL} OUTER JOIN    
{table-reference | outer-join} ON search-condition  
```  
  
 où `table-reference` est un nom de table et `search-condition` est la condition de jointure que vous souhaitez utiliser pour les tables.  
  
 Par exemple :  
  
```  
SELECT Customers.CustID, Customers.Name, Orders.OrderID, Orders.Status   
   FROM {oj Customers LEFT OUTER JOIN   
      Orders ON Customers.CustID=Orders.CustID}   
   WHERE Orders.Status='OPEN'  
```  
  
 Les séquences d'échappement de jointure externe suivantes sont prises en charge par le pilote JDBC :  
  
-   Jointures externes gauches  
  
-   Jointures externes droites  
  
-   Jointures externes entières  
  
-   Jointures externes imbriquées  
  
## <a name="limit-escape-syntax"></a>Syntaxe d’échappement Limit  
  
> [!NOTE]  
>  La syntaxe d’échappement LIMIT est uniquement pris en charge par Microsoft JDBC Driver 4.2 (ou version ultérieure) pour SQL Server à l’aide de JDBC 4.1 ou version ultérieure.  
  
 La syntaxe d'échappement pour LIMIT est la suivante :  
  
```  
LIMIT <rows> [OFFSET <row offset>]  
```  
  
 La syntaxe d’échappement comporte deux parties : \< *lignes*> est obligatoire et spécifie le nombre de lignes à retourner. OFFSET et \< *décalage de lignes*> sont facultatifs et spécifiez le nombre de lignes à ignorer avant de commencer à retourner des lignes. Le pilote JDBC prend en charge uniquement la partie obligatoire en transformant la requête pour qu'elle utilise la clause TOP au lieu de LIMIT. SQL Server ne prend pas en charge la clause LIMIT. **Le pilote JDBC ne prend pas en charge le paramètre facultatif \<décalage de lignes > et le pilote lève une exception si elle est utilisée**.  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’instructions avec le pilote JDBC](../../connect/jdbc/using-statements-with-the-jdbc-driver.md)  
  
  
