---
title: DECLARE CURSOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DECLARE_CURSOR_TSQL
- CURSOR_TSQL
- DECLARE CURSOR
- CURSOR
dev_langs:
- TSQL
helpviewer_keywords:
- DECLARE CURSOR statement
- cursors [SQL Server], attributes
- local cursors [SQL Server]
- nesting cursors
- Transact-SQL cursors, attributes
- global cursors [SQL Server]
ms.assetid: 5a3a27aa-03e8-4c98-a27e-809282379b21
caps.latest.revision: 51
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 42e3ae8b426b7230cf8cfee4be68838792d30318
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="declare-cursor-transact-sql"></a>DECLARE CURSOR (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Définit les attributs d'un curseur [!INCLUDE[tsql](../../includes/tsql-md.md)], notamment s'il permet ou non le défilement, et la requête utilisée pour créer le jeu de résultats sur lequel le curseur opère. La syntaxe d'une instruction DECLARE CURSOR peut utiliser à la fois la syntaxe ISO et un jeu d'extensions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ISO Syntax  
DECLARE cursor_name [ INSENSITIVE ] [ SCROLL ] CURSOR   
     FOR select_statement   
     [ FOR { READ ONLY | UPDATE [ OF column_name [ ,...n ] ] } ]  
[;]  
Transact-SQL Extended Syntax  
DECLARE cursor_name CURSOR [ LOCAL | GLOBAL ]   
     [ FORWARD_ONLY | SCROLL ]   
     [ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]   
     [ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]   
     [ TYPE_WARNING ]   
     FOR select_statement   
     [ FOR UPDATE [ OF column_name [ ,...n ] ] ]  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *cursor_name*  
 Nom du curseur côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)] défini. *cursor_name* doit suivre les règles applicables aux identificateurs.  
  
 INSENSITIVE  
 Définit un curseur qui effectue une copie temporaire des données qu'il doit utiliser. Toutes les réponses aux requêtes destinées au curseur sont effectuées à partir de cette table temporaire dans **tempdb**. Par conséquent, les modifications apportées aux tables de base de données ne sont pas répercutées dans les données retournées par les extractions de ce curseur, et ce dernier n’accepte pas de modifications. Si, lors de l'utilisation de la syntaxe ISO, l'option INSENSITIVE est omise, les suppressions et les mises à jour validées effectuées (par n'importe quel utilisateur) dans les tables sous-jacentes sont reflétées dans les extractions ultérieures.  
  
 SCROLL  
 Spécifie que toutes les fonctions d'extraction (FIRST, LAST, PRIOR, NEXT, RELATIVE, ABSOLUTE) sont disponibles. Si SCROLL n'est pas spécifié dans une instruction DECLARE CURSOR ISO, seule la fonction NEXT est prise en charge. SCROLL ne peut pas être spécifié si FAST_FORWARD l'est également.  
  
 *select_statement*  
 Instruction SELECT standard qui définit le jeu de résultats du curseur. Les mots clés FOR BROWSE et INTO ne sont pas autorisés dans l’instruction *select_statement* d’une déclaration de curseur.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit implicitement le curseur en un autre type si les clauses de l’instruction *select_statement* sont incompatibles avec la fonctionnalité du type de curseur demandé.  
  
 READ ONLY  
 Interdit les mises à jour par l'intermédiaire de ce curseur. Le curseur ne peut pas être référencé dans une clause WHERE CURRENT OF dans une instruction UPDATE ou DELETE. Cette option remplace la possibilité par défaut de mise à jour d'un curseur.  
  
 UPDATE [OF *column_name* [**,**...*n*]]  
 Définit les colonnes qui peuvent être mises à jour par le curseur. Si OF *column_name* [**,**..*.n*] est spécifié, seules les colonnes indiquées permettent les modifications. Si vous spécifiez UPDATE sans liste de colonnes, toutes les colonnes peuvent être mises à jour.  
  
 *cursor_name*  
 Nom du curseur côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)] défini. *cursor_name* doit suivre les règles applicables aux identificateurs.  
  
 LOCAL  
 Spécifie que l'étendue du curseur est locale pour le traitement d'instructions, la procédure stockée ou le déclencheur dans lequel il a été créé. Le nom du curseur n’est valide que dans cette étendue. Le curseur peut être référencé par des variables de curseur locales dans le traitement, la procédure stockée ou le déclencheur, ou bien par un paramètre OUTPUT d'une procédure stockée. Un paramètre OUTPUT est utilisé pour transmettre le curseur local au traitement, à la procédure stockée ou au déclencheur effectuant l'appel qui peut affecter le paramètre à une variable de curseur pour référencer le curseur à la fin de la procédure stockée. Le curseur est désalloué implicitement à la fin du traitement, de la procédure stockée ou du déclencheur à moins d'avoir été renvoyé dans un paramètre OUTPUT. S'il a été renvoyé dans un paramètre OUTPUT, le curseur est désalloué lorsque la dernière variable qui y fait référence est désallouée, ou lorsqu'il est hors de portée.  
  
 GLOBAL  
 Spécifie que l'étendue du curseur est globale pour la connexion. Toute procédure stockée ou tout lot exécuté par la connexion peut faire référence au nom du curseur. Le curseur n'est désalloué implicitement qu'au moment de la déconnexion.  
  
> [!NOTE]  
>  Si aucun des paramètres GLOBAL et LOCAL n’est spécifié, la valeur par défaut est définie par la configuration de l’option de base de données **default to local cursor**.  
  
 FORWARD_ONLY  
 Spécifie que le curseur peut seulement défiler de la première à la dernière ligne. Dans ce cas, FETCH NEXT est la seule fonction d'extraction prise en charge. Si vous spécifiez FORWARD_ONLY sans les mots clés STATIC, KEYSET ou DYNAMIC, le curseur est implémenté en tant que curseur dynamique. Si vous ne spécifiez ni FORWARD_ONLY ni SCROLL, FORWARD_ONLY est choisi par défaut, sauf si les mots clés STATIC, KEYSET ou DYNAMIC sont spécifiés. Les curseurs STATIC, KEYSET et DYNAMIC ont par défaut la valeur SCROLL. Contrairement aux interfaces de programmation d'application (API) de base de données telles que ODBC et ADO, FORWARD_ONLY est pris en charge avec les curseurs [!INCLUDE[tsql](../../includes/tsql-md.md)] STATIC, KEYSET et DYNAMIC.  
  
 STATIC  
 Définit un curseur qui effectue une copie temporaire des données qu'il doit utiliser. Toutes les réponses aux requêtes destinées au curseur sont effectuées à partir de cette table temporaire dans **tempdb**. Par conséquent, les modifications apportées aux tables de base de données ne sont pas répercutées dans les données retournées par les extractions de ce curseur, et ce dernier n’accepte pas de modifications.  
  
 KEYSET  
 Spécifie que l'appartenance au curseur et l'ordre des lignes sont fixés lors de l'ouverture du curseur. L’ensemble des clés qui identifient de manière unique les lignes est créé dans une table **tempdb** nommée **keyset**.  
  
> [!NOTE]  
>  Si la requête fait référence à au moins une table sans index unique, le curseur de jeu de clés est converti en curseur statique.  
  
 Les modifications apportées aux valeurs non-clés dans les tables de la base, que ce soit celles effectuées par le propriétaire du curseur ou celles validées par les autres utilisateurs, sont visibles lorsque le propriétaire parcourt le curseur. Les insertions effectuées par d'autres utilisateurs ne sont pas visibles (les insertions ne peuvent être réalisées via un curseur côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)]). Si vous supprimez une ligne, une tentative d’extraction de la ligne retourne une valeur -2 pour @@FETCH_STATUS. Les mises à jour de valeurs de clés effectuées hors du curseur sont semblables à la suppression de l'ancienne ligne suivie de l'insertion d'une nouvelle. La ligne avec les nouvelles valeurs n’est pas visible et les tentatives d’extraction avec les anciennes valeurs retournent la valeur -2 pour @@FETCH_STATUS. Les nouvelles valeurs sont visibles si la mise à jour est effectuée via le curseur en spécifiant la clause WHERE CURRENT OF.  
  
 DYNAMIC  
 Définit un curseur qui reflète toutes les modifications de données apportées aux lignes dans son jeu de résultats lorsque vous faites défiler le curseur. Les valeurs des données, l'ordre et l'appartenance aux lignes peuvent changer à chaque extraction. L'option d'extraction ABSOLUTE n'est pas prise en charge par les curseurs dynamiques.  
  
 FAST_FORWARD  
 Spécifie un curseur FORWARD_ONLY, READ_ONLY pour lequel les optimisations de performances sont activées. FAST_FORWARD ne peut pas être spécifié si SCROLL ou FOR_UPDATE le sont également.  
  
> [!NOTE]  
>  FAST_FORWARD et FORWARD_ONLY peuvent être utilisés dans la même instruction DECLARE CURSOR.  
  
 READ_ONLY  
 Interdit les mises à jour par l'intermédiaire de ce curseur. Le curseur ne peut pas être référencé dans une clause WHERE CURRENT OF dans une instruction UPDATE ou DELETE. Cette option remplace la possibilité par défaut de mise à jour d'un curseur.  
  
 SCROLL_LOCKS  
 Spécifie que la réussite des mises à jour ou des suppressions positionnées effectuées via le curseur est garantie. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrouille les lignes lorsqu’elles sont lues dans le curseur pour vérifier leur disponibilité lors des modifications ultérieures. SCROLL_LOCKS ne peut pas être spécifié si FAST_FORWARD ou STATIC l'est également.  
  
 OPTIMISTIC  
 Spécifie que les mises à jour ou les suppressions positionnées effectuées via le curseur ne réussissent pas si la ligne a été mise à jour depuis qu'elle a été lue dans le curseur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne verrouille pas les lignes à mesure qu'elles sont lues dans le curseur. Il utilise à la place des comparaisons des valeurs de la colonne **timestamp**, ou une valeur de somme de contrôle si la table n’a pas de colonne **timestamp**, pour déterminer si la ligne a été modifiée après avoir été lue dans le curseur. Si la ligne a été modifiée, la mise à jour ou la suppression positionnée que vous avez tentée échoue. OPTIMISTIC ne peut pas être spécifié si vous avez déjà spécifié FAST_FORWARD.  
  
 TYPE_WARNING  
 Indique qu'un message d'avertissement est envoyé au client lorsque le curseur est converti implicitement du type demandé vers un autre type.  
  
 *select_statement*  
 Instruction SELECT standard qui définit le jeu de résultats du curseur. Les mots clés COMPUTE, COMPUTE BY, FOR BROWSE et INTO ne sont pas autorisés dans l’instruction *select_statement* d’une déclaration de curseur.  
  
> [!NOTE]  
>  Vous pouvez utiliser un indicateur de requête dans une déclaration de curseur ; cependant, si vous utilisez également la clause FOR UPDATE OF, spécifiez OPTION (*query_hint*) après FOR UPDATE OF.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit implicitement le curseur en un autre type si les clauses de l’instruction *select_statement* sont incompatibles avec la fonctionnalité du type de curseur demandé. Pour plus d'informations, consultez la rubrique Conversions implicites de curseur.  
  
 FOR UPDATE [OF *column_name* [**,**...*n*]]  
 Définit les colonnes qui peuvent être mises à jour par le curseur. Si OF *column_name* [**,**...*n*] est fourni, seules les colonnes indiquées autorisent les modifications. Si vous spécifiez UPDATE sans liste de colonnes, toutes les colonnes peuvent être mises à jour, sauf si l'option de concurrence READ_ONLY a été spécifiée.  
  
## <a name="remarks"></a>Notes   
 DECLARE CURSOR définit les attributs d'un curseur [!INCLUDE[tsql](../../includes/tsql-md.md)], notamment s'il permet ou non le défilement, et la requête utilisée pour créer le jeu de résultats sur lequel le curseur opère. L'instruction OPEN remplit le jeu de résultats tandis que l'instruction FETCH renvoie une ligne à partir de ce jeu de résultats. L'instruction CLOSE libère le jeu de résultats actuel associé au curseur. L'instruction DEALLOCATE libère les ressources utilisées par le curseur.  
  
 Le premier format de l'instruction DECLARE CURSOR utilise la syntaxe ISO pour déclarer le comportement du curseur. Le second format utilise les extensions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui permettent de définir les curseurs en utilisant les mêmes types de curseur que ceux des fonctions de curseur API de base de données ODBC ou ADO.  
  
 Vous ne pouvez pas utiliser les deux formats simultanément. Si vous spécifiez les mots clés SCROLL ou INSENSITIVE avant le mot clé CURSOR, vous ne pouvez pas insérer de mot clé entre les mots clés CURSOR et FOR *select_statement*. Si vous spécifiez un mot clé entre les mots clés CURSOR et FOR *select_statement*, vous ne pouvez pas spécifier SCROLL ou INSENSITIVE avant le mot clé CURSOR.  
  
 Si une instruction DECLARE CURSOR utilisant la syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] ne spécifie pas READ_ONLY, OPTIMISTIC ou SCROLL_LOCKS, par défaut les valeurs sont comme suit :  
  
-   si l'instruction SELECT ne prend pas en charge les mises à jour (autorisations insuffisantes, accès à des tables éloignées ne prenant pas en charge les mises à jour, etc.), le curseur prend la valeur READ_ONLY ;  
  
-   les curseurs STATIC et FAST_FORWARD ont par défaut la valeur READ-ONLY ;  
  
-   les curseurs DYNAMIC et KEYSET ont par défaut la valeur OPTIMISTIC.  
  
 Les noms de curseurs peuvent uniquement être référencés par d'autres instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ils ne peuvent pas être référencés par des fonctions API de base de données. Ainsi, après la déclaration d'un curseur, le nom du curseur ne peut pas être référencé par des fonctions ou méthodes OLE DB, ODBC ou ADO. Les lignes du curseur ne peuvent pas être extraites à l'aide des fonctions ou méthodes d'extraction des interfaces de programmation d'application (API) mais uniquement au moyen d'instructions FETCH [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Une fois un curseur déclaré, les procédures stockées système suivantes peuvent être utilisées pour déterminer ses caractéristiques.  
  
|Procédures stockées système|Description|  
|------------------------------|-----------------|  
|**sp_cursor_list**|Renvoie une liste des curseurs actuellement visibles par la connexion et leurs attributs.|  
|**sp_describe_cursor**|Décrit les attributs d'un curseur. Indique par exemple s'il s'agit d'un curseur de défilement avant uniquement ou d'un curseur de défilement.|  
|**sp_describe_cursor_columns**|Décrit les attributs des colonnes contenues dans le jeu de résultats du curseur.|  
|**sp_describe_cursor_tables**|Décrit les tables de base auxquelles accède le curseur.|  
  
 Les variables peuvent être utilisées dans l’instruction *select_statement* qui déclare un curseur. Les valeurs de variable de curseur ne changent pas après la déclaration d'un curseur.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations DECLARE CURSOR sont octroyées par défaut à tout utilisateur qui a des autorisations SELECT sur les vues, les tables et les colonnes utilisées par le curseur.
 
## <a name="limitations-and-restrictions"></a>Limitations et restrictions

Vous ne pouvez pas utiliser de curseurs ou de déclencheurs sur une table avec un index cluster columnstore. Cette restriction ne s’applique pas aux index columnstore non-cluster. En effet, vous pouvez utiliser des curseurs et des déclencheurs sur une table avec un index columnstore non-cluster. 
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-simple-cursor-and-syntax"></a>A. Utilisation d'un curseur et d'une syntaxe simples  

Le jeu de résultats généré à l'ouverture du curseur ci-après contient toutes les lignes et toutes les colonnes de la table. Ce curseur peut être mis à jour, et toutes les mises à jour et les suppressions sont représentées sous la forme d'extractions effectuées sur ce curseur. `FETCH NEXT` est la seule extraction disponible car l'option `SCROLL` n'a pas été spécifiée.  

  
```  
DECLARE vend_cursor CURSOR  
    FOR SELECT * FROM Purchasing.Vendor  
OPEN vend_cursor  
FETCH NEXT FROM vend_cursor;  
```  
  
### <a name="b-using-nested-cursors-to-produce-report-output"></a>B. Utilisation de curseurs imbriqués pour la production d'un rapport  
 L'exemple suivant montre comment les curseurs peuvent être imbriqués pour produire des rapports complexes. Le curseur interne est déclaré pour chaque fournisseur.  
  
```  
SET NOCOUNT ON;  
  
DECLARE @vendor_id int, @vendor_name nvarchar(50),  
    @message varchar(80), @product nvarchar(50);  
  
PRINT '-------- Vendor Products Report --------';  
  
DECLARE vendor_cursor CURSOR FOR   
SELECT VendorID, Name  
FROM Purchasing.Vendor  
WHERE PreferredVendorStatus = 1  
ORDER BY VendorID;  
  
OPEN vendor_cursor  
  
FETCH NEXT FROM vendor_cursor   
INTO @vendor_id, @vendor_name  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    PRINT ' '  
    SELECT @message = '----- Products From Vendor: ' +   
        @vendor_name  
  
    PRINT @message  
  
    -- Declare an inner cursor based     
    -- on vendor_id from the outer cursor.  
  
    DECLARE product_cursor CURSOR FOR   
    SELECT v.Name  
    FROM Purchasing.ProductVendor pv, Production.Product v  
    WHERE pv.ProductID = v.ProductID AND  
    pv.VendorID = @vendor_id  -- Variable value from the outer cursor  
  
    OPEN product_cursor  
    FETCH NEXT FROM product_cursor INTO @product  
  
    IF @@FETCH_STATUS <> 0   
        PRINT '         <<None>>'       
  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
  
        SELECT @message = '         ' + @product  
        PRINT @message  
        FETCH NEXT FROM product_cursor INTO @product  
        END  
  
    CLOSE product_cursor  
    DEALLOCATE product_cursor  
        -- Get the next vendor.  
    FETCH NEXT FROM vendor_cursor   
    INTO @vendor_id, @vendor_name  
END   
CLOSE vendor_cursor;  
DEALLOCATE vendor_cursor;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [@@FETCH_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/fetch-status-transact-sql.md)   
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [Curseurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
