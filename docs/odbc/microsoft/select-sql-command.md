---
title: Sélectionnez - commande SQL | Documents Microsoft
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
- select [ODBC]
ms.assetid: 2149c3ca-3a71-446d-8d53-3d056e2f301a
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7d3803c1e51a21e8006994db5ba89f9b2cb41ad6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="select---sql-command"></a>Sélectionnez - commande SQL
Récupère les données à partir d’une ou plusieurs tables.  
  
 Le pilote ODBC Visual FoxPro prend en charge la syntaxe du langage Visual FoxPro native pour cette commande. Pour plus d’informations spécifiques au pilote, consultez **pilote notes**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SELECT [ALL | DISTINCT]  
   [Alias.] Select_Item [AS Column_Name]  
   [, [Alias.] Select_Item [AS Column_Name] ...]   
FROM [DatabaseName!]Table [Local_Alias]  
   [, [DatabaseName!]Table [Local_Alias] ...]   
[WHERE JoinCondition [AND JoinCondition  
…]  
   [AND | OR FilterCondition [AND | OR FilterCondition ...]]]  
[GROUP BY GroupColumn [, GroupColumn ...]]  
[HAVING FilterCondition]  
[UNION [ALL] SELECTCommand]  
[ORDER BY Order_Item [ASC | DESC] [, Order_Item [ASC | DESC] ...]]  
```  
  
## <a name="arguments"></a>Arguments  
  
> [!NOTE]  
>  A *sous-requête*, dénommé dans les arguments suivants, est une instruction SELECT dans une instruction SELECT et doit être mis entre parenthèses. Vous pouvez avoir jusqu'à deux sous-requêtes au même niveau (ne pas imbriquée) dans la clause WHERE. (Consultez la section des arguments). Les sous-requêtes peuvent contenir plusieurs conditions de jointure.  
  
 [Toutes les &#124; DISTINCT]   [*Alias*.] *Select_Item* [AS *Column_Name*] [, [*Alias*.] *Select_Item* [AS *Column_Name*]...]  
 La clause SELECT spécifie les champs, les constantes et les expressions qui sont affichées dans les résultats de requête.  
  
 Par défaut, tous les affiche toutes les lignes dans les résultats de requête.  
  
 DISTINCT exclut les doublons de toutes les lignes de résultats de la requête.  
  
> [!NOTE]  
>  Vous pouvez utiliser DISTINCT qu’une seule fois par la clause SELECT.  
  
 *Alias*. qualifie les noms d’élément correspondant. Chaque élément que vous spécifiez avec *Select_Item* génère une colonne des résultats de requête. Si deux ou plusieurs éléments ont le même nom, incluez l’alias de table et un point avant le nom de l’élément pour empêcher des colonnes d’être dupliqué.  
  
 *Select_Item* spécifie un élément à inclure dans les résultats de requête. Un élément peut être une des opérations suivantes :  
  
-   Le nom d’un champ d’une table dans la clause FROM.  
  
-   Une constante indiquant que la même valeur de constante doit s’afficher dans chaque ligne de résultats de la requête.  
  
-   Une expression qui peut être le nom d’une fonction définie par l’utilisateur.  
  
 **Fonctions définies par l’utilisateur avec SELECT**  
  
 Bien que l’utilisation de fonctions définies par l’utilisateur dans la clause SELECT a des avantages, vous devez également envisager les restrictions suivantes :  
  
-   La vitesse des opérations effectuées avec SELECT peut être limitée par la vitesse à laquelle ces fonctions définies par l’utilisateur sont exécutées. Les manipulations de volume élevé impliquant des fonctions définies par l’utilisateur peuvent être mieux exécutées à l’aide des API et des fonctions définies par l’utilisateur écrites en langage C ou assembly.  
  
-   La seule méthode fiable pour transmettre des valeurs à des fonctions définies par l’utilisateur appelées à partir de SELECT est par la liste d’arguments passée à la fonction lorsqu’elle est appelée.  
  
-   Même si vous testez et découvrez une manipulation censé interdite qui fonctionne correctement dans une version spécifique de FoxPro, il n’existe aucune garantie qu’il continue de fonctionner dans les versions ultérieures.  
  
 En dehors de ces restrictions, les fonctions définies par l’utilisateur sont acceptables dans la clause SELECT. Toutefois, n’oubliez pas que l’utilisation de SELECT peut ralentir les performances.  
  
 Les fonctions de champ suivantes sont disponibles pour une utilisation avec un élément sélectionné est un champ ou une expression impliquant un champ :  
  
-   AVG (*Select_Item*) : calcule la moyenne une colonne de données numériques.  
  
-   NOMBRE (*Select_Item*) : compte le nombre de sélectionner des éléments dans une colonne. Count compte le nombre de lignes dans le résultat de la requête.  
  
-   MIN (*Select_Item*) : détermine la plus petite valeur de *Select_Item* dans une colonne.  
  
-   MAX (*Select_Item*) : détermine la plus grande valeur de *Select_Item* dans une colonne.  
  
-   SUM (*Select_Item*), des totaux d’une colonne de données numériques.  
  
 Vous ne pouvez pas imbriquer des fonctions de champ.  
  
 AS *nom_colonne*  
 Spécifie l’en-tête d’une colonne dans la sortie de la requête. Cela est utile lorsque *Select_Item* est une expression ou contient un champ de fonction et que vous souhaitez donner un nom explicite à la colonne. *Column_Name* peut être une expression, mais ne peut pas contenir des caractères (par exemple, des espaces) qui ne sont pas autorisées dans les noms de champ de table.  
  
 À partir de [*DatabaseName*!] *Table* [*Local_Alias*] [, [*DatabaseName*!] *Table* [*Local_Alias*]...]  
 Répertorie les tables qui contiennent les données que la requête récupère. Si aucune table n’est ouverte, Visual FoxPro affiche le **ouvrir** boîte de dialogue afin que vous pouvez spécifier l’emplacement du fichier. Une fois qu’il a été ouvert, la table reste ouverte après que la requête est terminée.  
  
 *DatabaseName*! Spécifie le nom d’une base de données autre que celui spécifié avec la source de données. Vous devez inclure le nom de la base de données qui contient la table si la base de données n’est pas spécifié avec la source de données. Inclure le séparateur de point d’exclamation ( !) après le nom de la base de données et avant le nom de table.  
  
 *Local_Alias* spécifie un nom temporaire pour la table nommée dans *Table*. Si vous spécifiez un alias local, vous devez utiliser l’alias local au lieu du nom de table dans l’ensemble de l’instruction SELECT. L’alias local n’affecte pas l’environnement Visual FoxPro.  
  
 OÙ *JoinCondition* [AND *JoinCondition* ...]    [AND &#124; ou *FilterCondition* [AND &#124; ou *FilterCondition* ...]]  
 Indique à Visual FoxPro pour inclure uniquement certains enregistrements dans les résultats de requête. OÙ est nécessaire pour récupérer des données provenant de plusieurs tables.  
  
 *JoinCondition* spécifie les champs qui lient les tables dans la clause FROM. Si vous incluez plusieurs tables dans une requête, vous devez spécifier une condition de jointure pour chaque table après la première.  
  
> [!IMPORTANT]  
>  Considérez les points suivants lorsque vous créez des conditions de jointure :  
  
-   Si vous incluez des deux tables dans une requête et que vous ne spécifiez pas une condition de jointure, tous les enregistrements de la première table sont joint à tous les enregistrements de la seconde table tant que les conditions du filtre. Une telle requête peut produire des résultats relativement longue.  
  
-   Soyez prudent lorsque vous joignez des tables avec des champs vides car Visual FoxPro correspond aux champs vides. Par exemple, si vous joignez sur la table CUSTOMER. Code postal et de facture. Code postal et si le client contient 100 codes postaux vides et facture 400 codes postaux vides, le résultat de la requête contient 40 000 enregistrements supplémentaires résultant les champs vides. Utilisez le **() vide** fonction pour éliminer les enregistrements vides à partir de la sortie de la requête.  
  
-   Vous devez utiliser l’opérateur AND pour se connecter plusieurs conditions de jointure. Chaque condition de jointure a la forme suivante :  
  
     *Nom_champ1 comparaison Nom_champ2*  
  
     *Nom_champ1* est le nom d’un champ d’une table, *Nom_champ2* est le nom d’un champ à partir d’une autre table, et *comparaison* est un des opérateurs décrits dans le tableau suivant.  
  
|Opérateur|Comparaison|  
|--------------|----------------|  
|=|Égal à|  
|==|Exactement égal à|  
|LIKE|SEMBLABLE À SQL|  
|<>, !=, #|Non égal|  
|>|Plus de|  
|>=|Supérieur ou égal à|  
|<|Inférieur à|  
|<=|Inférieur ou égal à|  
  
 Lorsque vous utilisez l’opérateur = avec des chaînes, il se comporte différemment, selon le paramètre de SET ANSI. Lorsque SET ANSI est définie sur OFF, Visual FoxPro traite les comparaisons de chaînes d’une manière familière aux utilisateurs de Xbase. Lorsque SET ANSI est définie sur ON, Visual FoxPro suit les normes ANSI pour les comparaisons de chaînes. Consultez [SET ANSI](../../odbc/microsoft/set-ansi-command.md) et [SET EXACT](../../odbc/microsoft/set-exact-command.md) pour plus d’informations sur la façon dont Visual FoxPro effectue des comparaisons de chaînes.  
  
 *FilterCondition* spécifie les critères que les enregistrements doivent respecter pour être inclus dans les résultats de requête. Vous pouvez inclure autant de conditions dans une requête de filtre que vous le souhaitez, leur connexion avec l’opérateur AND ou opérateur OR. Vous pouvez également utiliser l’opérateur NOT pour inverser la valeur d’une expression logique, ou vous pouvez utiliser **() vide** pour rechercher un champ vide. *FilterCondition* peut prendre l’une des formes dans les exemples suivants :  
  
 **Exemple 1** *Nom_champ1 comparaison Nom_champ2*  
  
 `customer.cust_id = orders.cust_id`  
  
 **Exemple 2** *Expression de comparaison de FieldName*  
  
 `payments.amount >= 1000`  
  
 **Exemple 3** *FieldName comparaison* toutes les (*sous-requête*)  
  
 `company < ALL ;`  
  
 `(SELECT company FROM customer WHERE country = "USA")`  
  
 Lorsque la condition de filtre inclut tous, le champ doit satisfaire à la condition de comparaison de toutes les valeurs générées par la sous-requête avant son enregistrement est inclus dans les résultats de requête.  
  
 **Exemple 4** *FieldName comparaison* ANY &#124; SOME (*sous-requête*)  
  
 `company < ANY ;`  
  
 `(SELECT company FROM customer WHERE country = "USA")`  
  
 Lorsque la condition de filtre inclut tout ou partie, le champ doit satisfaire à la condition de comparaison pour au moins une des valeurs générées par la sous-requête.  
  
 L’exemple suivant vérifie si les valeurs dans le champ se trouvent dans une plage de valeurs spécifiée :  
  
 **Exemple 5** *FieldName* [NOT] entre *Start_Range* AND *End_Range*  
  
 `customer.postalcode BETWEEN 90000 AND 99999`  
  
 L’exemple suivant vérifie si au moins une ligne répond aux critères de la sous-requête. Lorsque la condition de filtre inclut EXISTS, la condition de filtre a la valeur True (. T.), sauf si la sous-requête renvoie le jeu vide.  
  
 **Exemple 6** [NOT] EXISTS (*sous-requête*)  
  
 `EXISTS ;`  
  
 `(SELECT * FROM orders WHERE customer.postalcode =`  
  
 `orders.postalcode)`  
  
 **Exemple 7** *FieldName* [NOT] IN *Value_Set*  
  
 `customer.postalcode NOT IN ("98052","98072","98034")`  
  
 Lorsque la condition de filtre inclut IN, le champ doit contenir une des valeurs avant son enregistrement est inclus dans les résultats de requête.  
  
 **Exemple 8** *FieldName* [NOT] IN (*sous-requête*)  
  
 `customer.cust_id IN ;`  
  
 `(SELECT orders.cust_id FROM orders WHERE orders.city="Seattle")`  
  
 Ici, le champ doit contenir une des valeurs retournées par la sous-requête avant son enregistrement est inclus dans les résultats de requête.  
  
 **Exemple 9** *FieldName* [NOT] LIKE *cExpression*  
  
 `customer.country NOT LIKE "USA"`  
  
 Recherche de cette condition de filtre pour chaque champ qui correspond aux *cExpression*. Vous pouvez utiliser le signe de pourcentage (%) et des caractères génériques de trait de soulignement (_) dans le cadre de *cExpression*. Le trait de soulignement représente un caractère unique inconnu dans la chaîne.  
  
 GROUP BY *GroupColumn* [, *GroupColumn* ...]  
 Groupes de lignes dans la requête en fonction des valeurs dans une ou plusieurs colonnes. *GroupColumn* peut prendre l’une des opérations suivantes :  
  
-   Le nom d’un champ de table standard.  
  
-   Un champ qui contient une fonction de champ SQL.  
  
-   Une expression numérique qui indique l’emplacement de la colonne dans la table de résultats. (Le numéro de colonne à l’extrême gauche est 1).  
  
 AVOIR *FilterCondition*  
 Spécifie une condition de filtre groupes doivent remplir pour être inclus dans les résultats de requête. HAVING doit être utilisé avec GROUP BY et peuvent inclure autant de conditions de filtre que vous le souhaitez, connectés par AND ou opérateur OR. Vous pouvez également utiliser ne pas pour inverser la valeur d’une expression logique.  
  
 *FilterCondition* ne peut pas contenir de sous-requête.  
  
 Une clause HAVING sans une clause GROUP BY se comporte comme une clause WHERE. Vous pouvez utiliser des alias locales et les fonctions de champ dans la clause HAVING. Utilisez une clause WHERE pour améliorer les performances si votre clause HAVING ne contient aucune fonction du champ.  
  
 [[ALL] de UNION *SELECTCommand*]  
 Combine les résultats finaux d’un sélectionner avec les résultats finaux de sélectionner un autre. Par défaut, UNION vérifie les résultats combinés et élimine les doublons de lignes. Utilisez des parenthèses pour combiner plusieurs clauses UNION.  
  
 UNION tous les empêche d’en éliminant les lignes en double dans les résultats combinés.  
  
 Clauses UNION suivent les règles suivantes :  
  
-   Vous ne pouvez pas utiliser UNION pour combiner des sous-requêtes.  
  
-   Les deux commandes SELECT doivent avoir le même nombre de colonnes dans la sortie de la requête.  
  
-   Chaque colonne dans les résultats de requête d’un sélectionner doit avoir le même type de données et la même largeur que la colonne correspondante dans l’autre instruction SELECT.  
  
-   Uniquement l’instruction SELECT finale peut avoir une clause ORDER BY, lequel doit faire référence aux colonnes de sortie par un nombre. Si une clause ORDER BY est incluse, il affecte le résultat terminé.  
  
 Vous pouvez également utiliser la clause UNION pour simuler une jointure externe.  
  
 Lorsque vous joignez deux tables dans une requête, uniquement les enregistrements avec les valeurs correspondantes dans les champs de jointure sont incluses dans la sortie. Si un enregistrement dans la table parente n’a pas d’un enregistrement correspondant dans la table enfant, l’enregistrement dans la table parente n’est pas inclus dans la sortie. Une jointure externe vous permet d’inclure tous les enregistrements dans la table parente dans la sortie, ainsi que les enregistrements correspondants dans la table enfant. Pour créer une jointure externe dans Visual FoxPro, vous devez utiliser une commande SELECT imbriquée, comme dans l’exemple suivant :  
  
```  
SELECT customer.company, orders.order_id, orders.emp_id ;  
FROM customer, orders ;  
WHERE customer.cust_id = orders.cust_id ;  
UNION ;  
SELECT customer.company, 0, 0 ;  
FROM customer ;  
WHERE customer.cust_id NOT IN ;  
(SELECT orders.cust_id FROM orders)  
```  
  
> [!NOTE]  
>  Assurez-vous que vous incluez l’espace qui précède immédiatement chaque point-virgule. Sinon, vous recevrez une erreur.  
  
 La section de la commande avant la clause UNION sélectionne les enregistrements des deux tables qui ont des valeurs correspondantes. Les sociétés de client qui n’ont pas de factures associées ne sont pas incluses. La section de la commande après la clause UNION sélectionne des enregistrements dans la table customer qui n’ont pas d’enregistrements correspondants dans la table orders.  
  
 En ce qui concerne la deuxième section de la commande, notez les points suivants :  
  
-   L’instruction SELECT entre parenthèses est traitée en premier. Cette instruction crée une sélection de tous les numéros de client dans la table orders.  
  
-   La clause WHERE recherche tous les numéros de client dans la table customer qui ne sont pas dans la table orders. Étant donné que la première section de la commande fourni toutes les sociétés qui avait un numéro de client dans la table orders, toutes les sociétés dans la table customer sont désormais incluses dans les résultats de requête.  
  
-   Étant donné que les structures des tables incluses dans une UNION doivent être identiques, il existe deux espaces réservés dans la deuxième instruction SELECT pour représenter *orders.order_id* et *orders.emp_id* à partir de la première instruction SELECT.  
  
    > [!NOTE]  
    >  Les espaces réservés doivent être du même type que les champs qu’ils représentent. Si le champ est un type de date, l’espace réservé doit être {/ /}. Si le champ est un champ de caractères, l’espace réservé doit être une chaîne vide (« »).  
  
 ORDER BY *Order_Item* [ASC &#124; DESC] [, *Order_Item* [ASC &#124; DESC]...]  
 Trie les résultats de requête basée sur les données dans une ou plusieurs colonnes. Chaque *Order_Item* doit correspondre à une colonne dans les résultats de requête et peut prendre l’une des opérations suivantes :  
  
-   Un champ dans une table de départ qui est également un élément sélectionné dans la clause SELECT principale (et non dans une sous-requête).  
  
-   Une expression numérique qui indique l’emplacement de la colonne dans la table de résultats. (La colonne de gauche est le nombre 1.)  
  
 ASC spécifie un ordre croissant des résultats de requête, en fonction de l’ordre ou les éléments et la valeur par défaut pour ORDER BY.  
  
 DESC spécifie un ordre décroissant pour les résultats de la requête.  
  
 Résultats de la requête s’affichent non triées, si vous ne spécifiez pas une commande avec ORDER BY.  
  
## <a name="remarks"></a>Notes  
 Sélectionnez est une commande SQL qui est intégrée à Visual FoxPro comme n’importe quelle autre commande Visual FoxPro. Lorsque vous utilisez, sélectionnez cette option pour présenter une requête, Visual FoxPro interprète la requête et récupère les données spécifiées à partir des tables. Vous pouvez créer une requête SELECT à partir de la fenêtre d’invite de commandes ou un programme Visual FoxPro (comme avec n’importe quelle autre commande Visual FoxPro).  
  
> [!NOTE]  
>  Sélectionnez ne respecte pas la condition de filtre actuel spécifiée par le filtre.  
  
## <a name="driver-remarks"></a>Section Notes de pilote  
 Lorsque votre application envoie l’instruction SQL ODBC SELECT à la source de données, le pilote ODBC Visual FoxPro convertit la commande dans la commande Sélectionner de Visual FoxPro sans traduction, sauf si la commande contient une séquence d’échappement ODBC. Les éléments entre dans une séquence d’échappement ODBC sont convertis en syntaxe de Visual FoxPro. Pour plus d’informations sur l’utilisation de ODBC des séquences d’échappement, consultez [fonctions d’heure et Date](../../odbc/microsoft/time-and-date-functions-visual-foxpro-odbc-driver.md) et dans le *de référence du programmeur ODBC de Microsoft*, consultez [les séquences d’échappement dans ODBC](../../odbc/reference/develop-app/escape-sequences-in-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [CRÉATION DE TABLE - SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [INSERTION - SQL](../../odbc/microsoft/insert-sql-command.md)   
 [SET ANSI](../../odbc/microsoft/set-ansi-command.md)   
 [VALEUR EXACTE](../../odbc/microsoft/set-exact-command.md)
