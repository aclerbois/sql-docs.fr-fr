---
title: Prise en charge d’Unicode et du classement | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: collations
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- binary collations [SQL Server]
- expression-level collations [SQL Server]
- Windows collations [SQL Server]
- globalization [SQL Server], about globalization
- supplementary character support
- code pages [SQL Server], about code pages
- collations [SQL Server], about collations
- Unicode [SQL Server], collations
- database-level collations [SQL Server]
- reading order
- column-level collations
- locales [SQL Server], about locales
- locales [SQL Server]
- code pages [SQL Server]
- SQL Server collations
- server-level collations [SQL Server]
ms.assetid: 92d34f48-fa2b-47c5-89d3-a4c39b0f39eb
caps.latest.revision: 46
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 25b36b25efbb7c99d3595da26587007f784bfc25
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34708037"
---
# <a name="collation-and-unicode-support"></a>Prise en charge d’Unicode et du classement
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Les classements dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournissent les règles de tri et les propriétés de respect de la casse et des accents pour vos données. Les classements utilisés avec les types de données character, tels que **char** et **varchar** , déterminent la page de codes et les caractères correspondants qui peuvent être représentés pour ce type de données. Qu’il s’agisse d’installer une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], de restaurer une sauvegarde de base de données ou de connecter un serveur à des bases de données clientes, vous devez bien comprendre les exigences en termes de paramètres régionaux, d’ordre de tri et de respect de la casse et des accents des données que vous utilisez. Pour répertorier les classements disponibles sur votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [sys.fn_helpcollations &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md).    
    
 Quand vous sélectionnez un classement pour votre serveur, base de données, colonne ou expression, vous assignez certaines caractéristiques à vos données qui affectent les résultats de nombreuses opérations dans la base de données. Par exemple, lorsque vous construisez une requête à l'aide d'ORDER BY, l'ordre de tri de votre jeu de résultats peut dépendre du classement appliqué à la base de données ou stipulé dans une clause COLLATE au niveau expression de la requête.    
    
 Pour exploiter au mieux la prise en charge des classements dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez comprendre les termes qui sont définis dans cette rubrique et la relation qu'ils entretiennent avec les caractéristiques de vos données.    
    
##  <a name="Terms"></a> Termes de classement    
    
-   [Classement](#Collation_Defn)    
    
-   [Paramètres régionaux](#Locale_Defn)    
    
-   [Page de codes](#Code_Page_Defn)    
    
-   [Ordre de tri](#Sort_Order_Defn)    
    
###  <a name="Collation_Defn"></a> Classement    
 Un classement désigne les modèles binaires qui représentent chaque caractère dans un jeu de données. Les classements déterminent également les règles de tri et de comparaison des données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le stockage d’objets ayant des classements différents dans une même base de données. Pour les colonnes non-Unicode, le paramètre de classement spécifie la page de codes pour les données et les caractères qui peuvent être représentés. Les données déplacées entre des colonnes non-Unicode doivent être converties de la page de codes source vers la page de codes de destination.    
    
 Le résultat d'une instruction[!INCLUDE[tsql](../../includes/tsql-md.md)] peut varier lorsque cette dernière est exécutée dans un contexte réunissant plusieurs bases de données dont chacune a un paramètre de classement différent. Dans la mesure du possible, choisissez un classement normalisé pour votre organisation. De cette manière, vous n'avez pas à spécifier explicitement le classement dans chaque caractère ou expression Unicode. Si vous devez utiliser des objets qui ont des paramètres de classement et de page de codes différents, codez vos requêtes conformément aux règles de priorité des classements. Pour plus d’informations, consultez [Priorité de classement (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md).    
    
 Les options associées à un classement sont le respect de la casse, le respect des accents, le respect du jeu de caractères Kana, le respect de la largeur et le respect du sélecteur de variante. Pour spécifier ces options, vous devez les ajouter au nom du classement. Par exemple, le classement `Japanese_Bushu_Kakusu_100_CS_AS_KS_WS` respecte la casse, les accents, le jeu de caractères Kana et la largeur. Autre exemple : le classement `Japanese_Bushu_Kakusu_140_CI_AI_KS_WS_VSS` ne respecte pas la casse, ne respecte pas les accents, respecte le jeu de caractères Kana, respecte la largeur et respecte le sélecteur de variante.  Le tableau suivant décrit le comportement associé à ces différentes options.    
    
|Option|Description|    
|------------|-----------------|    
|Respecter la casse (_CS)|Fait la distinction entre les majuscules et les minuscules. Si cette option est activée, les minuscules sont triées avant leurs équivalents majuscules. Si cette option n’est pas sélectionnée, le classement ne respecte pas la casse. Dans ce cas, SQL Server considère que les versions en majuscules et en minuscules des lettres sont identiques dans les opérations de tri. Vous pouvez explicitement sélectionner le non-respect de la casse en spécifiant _CI.|    
|Respecter les accents (_AS)|Fait la distinction entre les caractères accentués et non accentués. Par exemple, « a » n'est pas équivalent à « ấ ». Si cette option n’est pas sélectionnée, le classement ne respecte pas les accents. Dans ce cas, SQL Server considère que les versions accentuées et non accentuées des lettres sont identiques dans les opérations de tri. Vous pouvez explicitement sélectionner le non-respect des accents en spécifiant _AI.|    
|Respecter le jeu de caractères Kana (_KS)|Fait la distinction entre les deux types de caractères japonais Kana : Hiragana et Katakana. Si cette option n'est pas sélectionnée, le classement ne respecte pas les caractères Kana. Dans ce cas, SQL Server considère que les caractères Hiragana et Katakana sont identiques dans les opérations de tri. L'omission de cette option est le seul moyen de spécifier le non-respect du jeu de caractères Kana.|    
|Respecter la largeur (_WS)|Fait la différence entre les caractères pleine largeur et demi-largeur. Si cette option n'est pas sélectionnée, SQL Server considère que la représentation pleine largeur et demi-largeur d'un même caractère sont identiques dans les opérations de tri. L'omission de cette option est le seul moyen de spécifier le non-respect de la largeur.|    
|Respecter le sélecteur de variante (_VSS) | Fait la distinction entre différents sélecteurs de variante idéographiques dans les classements japonais Japanese_Bushu_Kakusu_140 et Japanese_XJIS_140 introduits dans [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]. Une séquence de variantes se compose d’un caractère de base et d’un sélecteur de variante supplémentaire. Si cette option _VSS n’est pas sélectionnée, le classement ne respecte pas le sélecteur de variante, et celui-ci n’est pas pris en compte dans la comparaison. Autrement dit, SQL Server considère comme identiques (à des fins de tri) les caractères basés sur le même caractère de base avec différents sélecteurs de variante. Voir aussi  [Unicode Ideographic Variation Database (Base de données de variantes idéographiques Unicode)](http://www.unicode.org/reports/tr37/). <br/><br/> Les classements qui respectent le sélecteur de variante (_VSS) ne sont pas pris en charge dans les index de recherche en texte intégral. Les index de recherche en texte intégral prennent uniquement en charge les options Respecter les accents (_AS), Respecter le jeu de caractères Kana (_KS) et Respecter la largeur (_WS). Les moteurs XML et CLR de SQL Server ne prennent pas en charge les sélecteurs de variante (_VSS).
    
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge les ensembles de classement suivants :    
    
 classements Windows    
 Les classements Windows définissent les règles de stockage des données de type caractère selon les paramètres régionaux système Windows associés. Pour un classement Windows, la comparaison de données non-Unicode est implémentée via le même algorithme que les données Unicode. Les règles de classement Windows de base spécifient l'alphabet ou la langue utilisée pour le tri du dictionnaire, et la page de codes utilisée pour stocker les données de type caractère non-Unicode. Les tris Unicode et non-Unicode sont compatibles avec les comparaisons de chaînes dans une version particulière de Windows. Les types de données sont ainsi cohérents dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce qui permet aux développeurs de trier les chaînes dans leurs applications en appliquant les mêmes règles que celles utilisées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Nom de classement Windows &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md).    
    
 Classements binaires    
 Les classements binaires trient les données en fonction de la séquence des valeurs codées qui sont définies par les paramètres régionaux et le type de données. Ils respectent la casse. Un classement binaire dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit les paramètres régionaux et la page de codes ANSI à utiliser. Cela applique un ordre de tri binaire. Parce qu'ils sont relativement simples, les classements binaires aident à améliorer les performances de l'application. Pour les types de données non-Unicode, les comparaisons de données sont basées sur les points de code qui sont définis dans la page de codes ANSI. Pour les données de type Unicode, les comparaisons de données se basent sur les points de code Unicode. Pour le classement binaire des types de données Unicode, les paramètres régionaux (la langue) ne sont pas pris en compte dans les tris de données. Par exemple, Latin_1_General_BIN et Japanese_BIN produisent des résultats de tri identiques quand ils sont utilisés avec des données Unicode.    
    
 Il existe deux types de classements binaires dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les anciens classements **BIN** et les nouveaux classements **BIN2** . Dans un classement **BIN2** tous les caractères sont triés en fonction de leurs points de code. Dans un classement **BIN** seul le premier caractère est trié selon le point de code, et les autres caractères sont triés en fonction de leurs valeurs d'octet. (En raison de l'architecture little endian de la plateforme Intel, les caractères de code Unicode sont toujours triés inversés par octet.)    
    
 classements SQL Server    
 Les classements (SQL_*)[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournissent la compatibilité d'ordre de tri avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les règles de tri du dictionnaire pour les données non-Unicode ne sont pas compatibles avec les routines de tri fournies par les systèmes d'exploitation Windows. Toutefois, le tri de données Unicode est compatible avec une version particulière de règles de tri Windows. Comme les classements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent des règles de comparaison différentes pour les données Unicode et non-Unicode, vous pouvez obtenir des résultats différents pour des comparaisons des mêmes données, selon le type de données sous-jacent. Pour plus d’informations, consultez [Nom du classement SQL Server &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md).    
    
> [!NOTE]    
>  Lorsque vous mettez à niveau une instance anglaise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez spécifier les classements (SQL_*) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour permettre la compatibilité avec les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existantes. Le classement par défaut d'une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] étant défini au cours de la procédure d'installation, assurez-vous de spécifier soigneusement les paramètres de classement dans les cas suivants :    
>     
>  -   Votre code d'application dépend du comportement des classements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] précédents.    
> -   Vous devez stocker des données de caractères de plusieurs langues.    
    
 Les paramétrages des classements sont pris en charge aux niveaux suivants d'une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:    
    
 Classements au niveau du serveur    
 Le classement du serveur par défaut est défini durant l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , et devient également le classement par défaut des bases de données système et de toutes les bases de données utilisateur. Notez que les classements Unicode ne peuvent pas être sélectionnés pendant la procédure d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , car ils ne sont pas pris en charge en tant que classements au niveau du serveur.    
    
 Lorsqu'un classement a été affecté au serveur, vous ne pouvez pas le modifier le classement, à moins d'exporter la totalité des objets et des données de la base de données, de reconstruire la base de données **master** et d'importer la totalité des objets et des données de la base de données. Au lieu de modifier le classement par défaut d'une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez spécifier le classement désiré au moment de la création d'une nouvelle base de données ou colonne de base de données.    
    
 Classements au niveau de la base de données    
 Lorsque vous créez ou modifiez une base de données, vous pouvez utiliser la clause COLLATE de l'instruction CREATE DATABASE ou ALTER DATABASE pour spécifier le classement par défaut de la base de données. Si aucun classement n'est spécifié, le classement du serveur est affecté à la base de données.    
    
 Vous ne pouvez pas modifier le classement des bases de données système, à moins de modifier le classement du serveur.    
    
 Le classement de la base de données est utilisé pour toutes les métadonnées de la base de données. Il constitue le classement par défaut pour l'ensemble des colonnes de chaîne, des objets temporaires, des noms de variable et des autres chaînes utilisées dans la base de données. Quand vous modifiez le classement d'une base de données utilisateur, des conflits de classement risquent de se produire quand des requêtes de la base de données accèdent à des tables temporaires. Les tables temporaires sont toujours stockées dans la base de données système **tempdb**, qui utilise le classement de l’instance. Les requêtes qui comparent les données caractères entre la base de données utilisateur et **tempdb** risquent d'échouer si les classements génèrent un conflit lors de l'évaluation des données caractères. Vous pouvez résoudre ce problème en spécifiant la clause COLLATE dans la requête. Pour plus d’informations, consultez [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md).    
    
 Classements au niveau des colonnes    
 Lorsque vous créez ou modifiez une table, vous pouvez spécifier des classements pour chaque colonne de chaîne de caractères à l'aide de la clause COLLATE. Si aucun classement n'est spécifié, le classement par défaut de la base de données est attribué à la colonne.    
    
 Classements au niveau de l'expression    
 Les classements au niveau de l'expression sont définis lors de l'exécution d'une instruction et ils affectent la façon dont l'ensemble de résultats est retourné. Cela permet aux résultats de tri ORDER BY d'être spécifiques aux paramètres régionaux. Utilisez une clause COLLATE telle que la suivante pour implémenter les classements au niveau de l'expression :    
    
```    
SELECT name FROM customer ORDER BY name COLLATE Latin1_General_CS_AI;    
```    
    
###  <a name="Locale_Defn"></a> Paramètres régionaux    
 Les paramètres régionaux sont un ensemble d'informations associées à un emplacement ou à une culture. Il peut s'agir du nom et de l'identificateur de la langue parlée, du script utilisé pour écrire la langue et des conventions culturelles. Les classements peuvent être associés à un ou plusieurs ensembles de paramètres régionaux. Pour plus d'informations, consultez [Locale IDs Assigned by Microsoft (en anglais)](http://msdn.microsoft.com/goglobal/bb964664.aspx).    
    
###  <a name="Code_Page_Defn"></a> Code Page    
 Une page de codes est le jeu ordonné de caractères d'un script donné dans lequel un index numérique (ou une valeur de point de code) est associé à chaque caractère. Une page de codes Windows est généralement appelée *jeu de caractères* ou *charset*. Les pages de codes permettent d'assurer la prise en charge des jeux de caractères et des dispositions du clavier utilisés par différents paramètres régionaux système Windows.     
###  <a name="Sort_Order_Defn"></a> Sort Order    
 L'ordre de tri spécifie comment sont triées les valeurs de données. Cela affecte les résultats de comparaison de données. Les données sont triées en utilisant les classements et peuvent être optimisées à l'aide des index.    
    
##  <a name="Unicode_Defn"></a> Prise en charge d’Unicode    
 Unicode est un standard en matière de correspondance de points de code avec des caractères. Comme il est conçu pour couvrir tous les caractères de toutes les langues du monde, il n'y a pas besoin de pages de codes différentes pour gérer des jeux de caractères différents. Si vous stockez des données de caractères de plusieurs langues, utilisez toujours les types de données Unicode (**nchar**, **nvarchar**et **ntext**) à la place des types de données non-Unicode (**char**, **varchar**et **text**).    
    
 Des limitations significatives sont associées aux types de données non-Unicode. C’est parce qu’un ordinateur non-Unicode ne peut utiliser qu’une seule page de codes. Vous pouvez bénéficier de gains de performances en utilisant Unicode parce qu'un moins grand nombre de conversions de page de codes est requis. Les classements Unicode doivent être sélectionnés individuellement au niveau de la base de données, de la colonne ou de l’expression parce qu’ils ne sont pas pris en charge au niveau du serveur.    
    
 Les pages de codes qu'utilise un client sont déterminées par les paramètres du système d'exploitation. Pour définir les pages de codes du client sur le système d'exploitation Windows, utilisez **Paramètres régionaux** dans le Panneau de configuration.    
    
 Lorsque vous déplacez des données d'un serveur vers un client, votre classement du serveur peut ne pas être reconnu par les pilotes de clients plus anciens. Cela peut se produire lorsque vous déplacez des données d'un serveur Unicode vers un client non-Unicode. La meilleure solution peut alors consister à mettre à niveau le système d'exploitation du client afin de mettre aussi à jour les classements du système sous-jacent. Si le client est équipé du logiciel client de base de données, vous pouvez envisager de lui appliquer une mise à jour du service.    
    
 Vous pouvez également essayer d'utiliser un autre classement pour les données du serveur. Choisissez un classement qui établit un mappage à une page de codes du client.    
    
 Pour utiliser les classements UTF-16 disponibles dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] afin d’améliorer la recherche et le tri de certains caractères Unicode (classements Windows uniquement), vous pouvez sélectionner l’un des classements (_SC) de caractères supplémentaires ou l’un des classements version 140.    
    
 Pour déterminer les problèmes qui sont liés à l'utilisation des types de données Unicode ou non-Unicode, testez votre scénario pour mesurer les écarts de performances dans votre environnement. Vous devez normaliser le classement utilisé sur les systèmes de votre organisation et déployer des serveurs et clients Unicode partout où vous le pouvez.    
    
 Dans de nombreuses situations, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interagit avec d’autres serveurs ou clients, et votre organisation peut utiliser plusieurs normes d’accès aux données entre les applications et les instances de serveur. Les clients[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont l'un des deux types principaux :    
    
-   Les**clients Unicode** qui utilisent OLE DB et ODBC (Open Database Connectivity) 3.7 ou version ultérieure.    
    
-   Les**clients non-Unicode** qui utilisent DB-Library et ODBC 3.6 ou version antérieure.    
    
 Le tableau suivant présente des informations sur l'utilisation des données multilingues avec diverses combinaisons de serveurs Unicode et non-Unicode.    
    
|Serveur|Client|Avantages ou restrictions|    
|------------|------------|-----------------------------|    
|Unicode|Unicode|Comme les données Unicode sont utilisées dans tout le système, ce scénario fournit les meilleures performances et la meilleure protection contre l’altération des données récupérées. C’est le cas avec ActiveX Data Objects (ADO), OLE DB et ODBC 3.7 ou version ultérieure.|    
|Unicode|Non-Unicode|Dans ce scénario, et surtout avec les connexions entre un serveur exécutant un système d'exploitation récent et un client exécutant une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ou un système d'exploitation plus ancien, il peut y avoir des limitations ou des erreurs lorsque vous déplacez des données vers un ordinateur client. Les données Unicode sur le serveur tentent d’établir un mappage à une page de codes correspondante sur le client non-Unicode afin de convertir les données.|    
|Non-Unicode|Unicode|Cette configuration n'est pas idéale pour l'utilisation de données multilingues. Vous ne pouvez pas écrire des données Unicode sur le serveur non-Unicode. Des problèmes peuvent survenir lorsque les données sont envoyées à des serveurs qui sont en dehors de la page de codes du serveur.|    
|Non-Unicode|Non-Unicode|Cette configuration est la plus limitée pour des données multilingues. Vous pouvez utiliser uniquement une seule page de codes.|    
    
##  <a name="Supplementary_Characters"></a> Caractères supplémentaires    
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit des types de données tels que **nchar** et **nvarchar** pour stocker des données Unicode. Ces types de données encodent le texte dans un format appelé *UTF-16*. Le Consortium Unicode alloue à chaque caractère un codepoint unique, qui est une valeur comprise entre 0x0000 et 0x10FFFF. Les caractères utilisés le plus souvent ont des valeurs codepoint qui correspondent à un mot de 16 bits en mémoire et sur le disque, mais les caractères dont les valeurs codepoint sont supérieures à 0xFFFF nécessitent deux mots de 16 bits consécutifs. Ces caractères sont appelés des *caractères supplémentaires*et les deux mots de 16 bits consécutifs sont appelés des *paires de substitution*.    
    
 À compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], vous pouvez utiliser une nouvelle famille de classements de caractères supplémentaires (_SC) avec les types de données **nchar**, **nvarchar** et **sql_variant**. Par exemple : `Latin1_General_100_CI_AS_SC` ou si vous utilisez un classement japonais, `Japanese_Bushu_Kakusu_100_CI_AS_SC`.    

 À compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], tous les nouveaux classements prennent en charge automatiquement les caractères supplémentaires.

 Si vous utilisez des caractères supplémentaires :    
    
-   Les caractères supplémentaires peuvent être utilisés dans des opérations de tri et de comparaison dans les versions de classement 90 ou versions supérieures.    
    
-   Tous les classements version 100 prennent en charge le tri linguistique avec les caractères supplémentaires.    
    
-   Les caractères supplémentaires ne sont pas utilisables dans les métadonnées, telles que les noms d'objets de base de données.    
    
-   Les bases de données qui utilisent des classements avec des caractères supplémentaires (\_SC) ne peuvent pas être activées pour la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. C’est parce que certaines des tables système et des procédures stockées créées pour la réplication utilisent le type de données **ntext** hérité qui ne prend pas en charge les caractères supplémentaires.  
    
-   L’indicateur SC peut s’appliquer aux éléments suivants :    
    
    -   Classements version 90    
    
    -   Classements version 100    
    
-   L’indicateur SC ne peut pas s’appliquer aux éléments suivants :    
    
    -   Classements Windows version 80 et sans version    
    
    -   Classements binaires BIN ou BIN2    
    
    -   Classements SQL\*    
    
    -   Classements version 140 (ces derniers n’ont pas besoin de l’indicateur SC, car ils prennent déjà en charge les caractères supplémentaires)    
    
 Le tableau suivant compare le comportement de quelques fonctions de chaîne et opérateurs de chaîne quand ils utilisent des caractères supplémentaires avec et sans classement sensible aux caractères supplémentaires :    
    
|Fonction de chaîne ou opérateur|Avec un classement sensible aux caractères supplémentaires|Sans classement sensible aux caractères supplémentaires|    
|---------------------------------|--------------------------|-----------------------------|    
|[CHARINDEX](../../t-sql/functions/charindex-transact-sql.md)<br /><br /> [LEN](../../t-sql/functions/len-transact-sql.md)<br /><br /> [PATINDEX](../../t-sql/functions/patindex-transact-sql.md)|La paire de substitution UTF-16 est comptée comme un codepoint unique.|La paire de substitution UTF-16 est comptée comme deux codepoints.|    
|[LEFT](../../t-sql/functions/left-transact-sql.md)<br /><br /> [REPLACE](../../t-sql/functions/replace-transact-sql.md)<br /><br /> [REVERSE](../../t-sql/functions/reverse-transact-sql.md)<br /><br /> [RIGHT](../../t-sql/functions/right-transact-sql.md)<br /><br /> [SUBSTRING](../../t-sql/functions/substring-transact-sql.md)<br /><br /> [STUFF](../../t-sql/functions/stuff-transact-sql.md)|Ces fonctions traitent chaque paire de substitution comme un codepoint unique et fonctionnent comme attendu.|Ces fonctions peuvent fractionner toutes les paires de substitution et conduire à des résultats inattendus.|    
|[NCHAR](../../t-sql/functions/nchar-transact-sql.md)|Retourne le caractère qui correspond à la valeur de codepoint Unicode spécifiée dans la plage 0 à 0x10FFFF. Si la valeur spécifiée se trouve dans la plage 0 à 0xFFFF, un seul caractère est retourné. Pour les valeurs plus élevées, la paire de substitution correspondante est retournée.|Une valeur supérieure à 0xFFFF retourne NULL au lieu du substitut correspondant.|    
|[UNICODE](../../t-sql/functions/unicode-transact-sql.md)|Retourne un codepoint UTF-16 dans la plage 0 à 0x10FFFF.|Retourne un codepoint UCS-2 dans la plage 0 à 0xFFFF.|    
|[Recherche de correspondance d’un seul caractère générique](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)<br /><br /> [Caractère générique - Caractères à ne pas faire correspondre](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)|Les caractères supplémentaires sont pris en charge pour toutes les opérations génériques.|Les caractères supplémentaires ne sont pas pris en charge pour ces opérations génériques. D'autres opérateurs génériques sont pris en charge.|    
    
##  <a name="GB18030"></a> Prise en charge du langage GB18030    
 GB18030 est une norme distincte utilisée en République populaire de Chine pour l'encodage des caractères chinois. Dans la norme GB18030, les caractères peuvent être encodés sur 1, 2 ou 4 octets de longueur. Pour prendre en charge les caractères encodés selon la norme GB18030,[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les reconnaît lorsqu'ils entrent dans le serveur en provenance d'une application côté client, puis les convertit et les stocke en mode natif en tant que caractères Unicode. Une fois stockés dans le serveur, ils sont traités en tant que caractères Unicode dans toutes les opérations suivantes. Vous pouvez utiliser n'importe quel classement chinois, de préférence la version 100 la plus récente. Tous les classements de niveau _100 prennent en charge le tri linguistique avec les caractères GB18030. Si les données incluent des caractères supplémentaires (paires de substitution), vous pouvez utiliser les classements SC disponibles dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] pour améliorer la recherche et le tri.    
    
##  <a name="Complex_script"></a> Prise en charge des scripts complexes    
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut prendre en charge l'entrée, le stockage, la modification et l'affichage de scripts complexes. Les scripts complexes sont notamment les suivants :    
    
-   Scripts qui associent l'utilisation de textes écrits de droite à gauche et de gauche à droite, par exemple les textes écrits en arabe et en anglais.    
-   Scripts dont les caractères changent de forme en fonction de leur position ou lorsqu'ils sont associés à d'autres caractères, par exemple les caractères arabes, indiens et thaï.    
-   Les langues telles que le thaï nécessitent des dictionnaires internes pour la reconnaissance des mots, car ces derniers ne sont pas séparés.    
    
Les applications de base de données qui interagissent avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent utiliser des contrôles qui prennent en charge les scripts complexes. Les contrôles Windows Form standard créés dans du code managé peuvent prendre en charge les scripts complexes.    

##  <a name="Japanese_Collations"></a> Classements japonais ajoutés dans  [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]
 
À compter de [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)], deux nouvelles familles de classement japonais sont prises en charge, avec les permutations de diverses options (\_CS, \_AS, \_KS, \_WS, \_VSS). 

Pour lister ces classements, vous pouvez interroger le moteur de base de données SQL Server :
``` 
SELECT Name, Description FROM fn_helpcollations()  
WHERE Name LIKE 'Japanese_Bushu_Kakusu_140%' OR Name LIKE 'Japanese_XJIS_140%'
``` 

Tous les nouveaux classements prenant automatiquement en charge les caractères supplémentaires, aucun d’eux n’a (ou ne requiert) l’indicateur SC.

Ces classements sont pris en charge dans les index de moteur de base de données, les tables optimisées en mémoire, les index columnstore et les modules compilés en mode natif.
    
##  <a name="Related_Tasks"></a> Tâches associées    
    
|Tâche|Rubrique|    
|----------|-----------|    
|Explique comment définir ou modifier le classement de l'instance de SQL Server.|[Définir ou modifier le classement du serveur](../../relational-databases/collations/set-or-change-the-server-collation.md)|    
|Explique comment définir ou modifier le classement d'une base de données utilisateur.|[Définir ou modifier le classement de la base de données](../../relational-databases/collations/set-or-change-the-database-collation.md)|    
|Explique comment définir ou modifier le classement d'une colonne dans la base de données.|[Définir ou modifier le classement des colonnes](../../relational-databases/collations/set-or-change-the-column-collation.md)|    
|Explique comment retourner des informations de classement au niveau du serveur, de la base de données ou de la colonne.|[Afficher des informations de classement](../../relational-databases/collations/view-collation-information.md)|    
|Décrit comment écrire des instructions Transact-SQL qui sont plus portables d’un langage à un autre ou qui prennent en charge plusieurs langages plus facilement.|[Rédiger des instructions Transact-SQL internationales](../../relational-databases/collations/write-international-transact-sql-statements.md)|    
|Explique comment modifier la langue des messages d'erreur et des paramètres relatifs à l'utilisation et l'affichage de la date, de l'heure et des devises.|[Définir une langue de session](../../relational-databases/collations/set-a-session-language.md)|    
    
##  <a name="Related_Content"></a> Contenu connexe    
 [SQL Server Best Practices Collation Change (Bonnes pratiques relatives au changement de classement dans SQL Server)](http://go.microsoft.com/fwlink/?LinkId=113891)    
    
 [SQL Server Best Practices Migration to Unicode (Bonnes pratiques relatives à la migration vers Unicode dans SQL Server)](http://go.microsoft.com/fwlink/?LinkId=113890)    
    
 [Site web du consortium Unicode](http://go.microsoft.com/fwlink/?LinkId=48619)    
    
## <a name="see-also"></a> Voir aussi    
 [Classements de base de données à relation contenant-contenu](../../relational-databases/databases/contained-database-collations.md)     
 [Choisir une langue lors de la création d’un index de recherche en texte intégral](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md)     
 [sys.fn_helpcollations &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)    
    
  

