---
title: Instructions de programmation (pilote ODBC pour SQL Server) | Documents Microsoft
ms.custom: ''
ms.date: 01/11/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 68a7b720ab2ab9a4280cc6d8a22a37cf0cad0f5d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="programming-guidelines"></a>Instructions de programmation

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Les fonctionnalités de programmation de la [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur macOS et Linux sont basées sur ODBC dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] Native Client ([SQL Server Native Client (ODBC)](http://go.microsoft.com/fwlink/?LinkID=134151)). [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] Native Client est basé sur ODBC dans Windows Data Access Components ([de référence du programmeur ODBC](http://go.microsoft.com/fwlink/?LinkID=45250)).  

Une application ODBC peut utiliser Multiple Active Result Sets (MARS) et autres [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] des fonctionnalités spécifiques en incluant `/usr/local/include/msodbcsql.h` après avoir inclus les en-têtes unixODBC (`sql.h`, `sqlext.h`, `sqltypes.h`, et `sqlucode.h`). Puis utilisez les mêmes noms symboliques pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)]-des éléments spécifiques que vous utilisez dans vos applications Windows ODBC.

## <a name="available-features"></a>Fonctionnalités disponibles  
Les sections suivantes à partir de la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] documentation Native Client pour ODBC ([SQL Server Native Client (ODBC)](http://go.microsoft.com/fwlink/?LinkID=134151)) sont valides lors de l’utilisation du pilote ODBC sur macOS et Linux :  

-   [Communication avec SQL Server (ODBC)](http://msdn.microsoft.com/library/ms131692.aspx)  
-   [Connexion et la requête prise en charge de délai d’attente](http://msdn.microsoft.com/library/ms130822.aspx)  
-   [Curseurs](http://msdn.microsoft.com/library/ms130794(SQL.110).aspx)  
-   [Date/heure améliorations (ODBC)](http://msdn.microsoft.com/library/bb677319.aspx)  
-   [Exécution de requêtes (ODBC)](http://msdn.microsoft.com/library/ms131677.aspx)  
-   [Gestion des erreurs et des messages](http://msdn.microsoft.com/library/ms131289.aspx)  
-   [Authentification Kerberos](http://msdn.microsoft.com/library/cc280459.aspx)  
-   [Types CLR volumineux définis par l’utilisateur (ODBC)](http://msdn.microsoft.com/library/bb677316.aspx)  
-   [Exécution de Transactions (ODBC) (à l’exception des transactions distribuées)](http://msdn.microsoft.com/library/ms131706.aspx)  
-   [Traitement des résultats (ODBC)](http://msdn.microsoft.com/library/ms130812.aspx)  
-   [Exécution de procédures stockées](http://msdn.microsoft.com/library/ms131440.aspx)
-   [Prise en charge des colonnes éparses (ODBC)](http://msdn.microsoft.com/library/cc280357.aspx)
-   [Chiffrement SSL](http://msdn.microsoft.com/library/ms131691.aspx)
-   [Tableau des paramètres table](https://docs.microsoft.com/en-us/sql/relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc)
-   [UTF-8 et UTF-16 pour l’API de commande et de données](http://msdn.microsoft.com/library/ff878241.aspx)
-   [Utilisation des fonctions de catalogue](http://msdn.microsoft.com/library/ms131490.aspx)  

## <a name="unsupported-features"></a>Fonctions non prises en charge

Les fonctionnalités suivantes n’ont pas été vérifiées fonctionne correctement dans cette version du pilote ODBC sur macOS et Linux :

-   Connexion de Cluster de basculement
-   [Résolution IP de réseau transparent](https://docs.microsoft.com/en-us/sql/connect/odbc/linux/using-transparent-network-ip-resolution) (avant le pilote ODBC 17)
-   [Suivi de pilote avancées](https://blogs.msdn.microsoft.com/mattn/2012/05/15/enabling-advanced-driver-tracing-for-the-sql-native-client-odbc-drivers/)

Les fonctionnalités suivantes ne sont pas disponibles dans cette version du pilote ODBC sur macOS et Linux : 

-   Les Transactions distribuées (attribut SQL_ATTR_ENLIST_IN_DTC n’est pas prise en charge)  
-   Mise en miroir de bases de données  
-   FILESTREAM  
-   Profilage des performances du pilote ODBC, présentés dans [SQLSetConnectAttr](http://go.microsoft.com/fwlink/?LinkId=234099)et les attributs de connexion liés aux performances suivants :  
    -   SQL_COPT_SS_PERF_DATA  
    -   SQL_COPT_SS_PERF_DATA_LOG  
    -   SQL_COPT_SS_PERF_DATA_LOG_NOW  
    -   SQL_COPT_SS_PERF_QUERY  
    -   SQL_COPT_SS_PERF_QUERY_INTERVAL  
    -   SQL_COPT_SS_PERF_QUERY_LOG  
-   SQLBrowseConnect  
-   Types d’intervalle C comme SQL_C_INTERVAL_YEAR_TO_MONTH (documentées dans [les identificateurs de Type de données et les descripteurs de](http://msdn.microsoft.com/library/ms716351(VS.85).aspx))
-   La valeur SQL_CUR_USE_ODBC de l’attribut SQL_ATTR_ODBC_CURSORS de la fonction SQLSetConnectAttr.

## <a name="character-set-support"></a>Prise en charge du jeu de caractères

Pour ODBC Driver 13 et 13.1, les données SQLCHAR doivent être UTF-8. Aucuns autres encodages ne sont pris en charge.

17 du pilote ODBC, les données SQLCHAR dans un des jeux/codages de caractères ci-dessous sont pris en charge :

|Nom| Description|
|-|-|
|UTF-8|Unicode|
|CP437|MS-DOS Latin des États-Unis|
|CP850|MS-DOS Latin 1|
|CP874|Latin/thaï|
|CP932|Japonais, Shift-JIS|
|CP936|Chinois simplifié GBK|
|CP949|Coréen, EUC-KR|
|CP950|Chinois traditionnel Big5|
|CP1251|Cyrillique|
|CP1253|Greek|
|CP1256|Arabe|
|CP1257|Balte|
|CP1258|Vietnamien|
|ISO-8859-1 / CP1252|Latin-1|
|ISO-8859-2 / CP1250|Latin-2|
|ISO-8859-3|Latin-3|
|ISO-8859-4|Latin-4|
|ISO-8859-5|Latin/cyrillique|
|ISO-8859-6|Latin/arabe|
|ISO-8859-7|Latin/grec|
|ISO-8859-8 / CP1255|Hébreu|
|ISO-8859-9 / CP1254|Turc|
|ISO-8859-13|Latin-7|
|ISO-8859-15|Latin-9|

Lors de la connexion, le pilote détecte les paramètres régionaux actuels du processus, dans qu'il est chargé. Si elle utilise un des encodages ci-dessus, le pilote utilise cet encodage pour les données SQLCHAR (caractère étroits) ; Sinon, la valeur par défaut UTF-8. Étant donné que tous les processus démarre dans les paramètres régionaux « C » par défaut (et par conséquent entraîner le pilote à la valeur par défaut au format UTF-8), si une application doit utiliser un des encodages ci-dessus, elle doit utiliser le **setlocale** afin de définir les paramètres régionaux de manière appropriée avant connexion ; spécifier les paramètres régionaux de votre choix de manière explicite, ou à l’aide d’une chaîne vide comme `setlocale(LC_ALL, "")` à utiliser les paramètres régionaux de l’environnement.

Par conséquent, dans un type Linux ou Mac environnement dont l’encodage est UTF-8, les utilisateurs de la mise à niveau de 17 du pilote ODBC de 13 ou 13.1 observera pas toutes les différences. Toutefois, les applications qui utilisent un encodage non UTF-8 dans la liste ci-dessus via `setlocale()` besoin d’utiliser ce codage pour les données vers/à partir du pilote au lieu d’UTF-8.

Les données SQLWCHAR doivent être au format UTF-16LE (Little Endian).

Lors de la liaison des paramètres d’entrée avec SQLBindParameter, si un caractère étroit SQL type tel que SQL_VARCHAR est spécifié, le pilote convertit les données fournies à partir du client d’encodage à la valeur par défaut (en général, page de codes 1252) [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] encodage. Pour les paramètres de sortie, le pilote convertit à partir de l’encodage spécifié dans les informations de classement associées aux données au client l’encodage. Toutefois, une perte de données est possible---convertit des caractères dans l’encodage de la source n’est pas représentables dans l’encodage cible à un point d’interrogation (« ? »).

Pour éviter cette perte de données lors de la liaison des paramètres d’entrée, spécifiez un type de caractère SQL Unicode comme SQL_NVARCHAR. Dans ce cas, le pilote convertit à partir du client de l’encodage UTF-16, ce qui peut représenter tous les caractères Unicode. En outre, la colonne cible ou le paramètre sur le serveur doit également être un type Unicode (**nchar**, **nvarchar**, **ntext**) ou l’autre avec un classement/encodage, qui peut représenter tous les caractères de la source de données d’origine. Pour éviter la perte de données avec les paramètres de sortie, spécifiez un type SQL Unicode et soit un Unicode type C (SQL_C_WCHAR), et que le pilote retourner des données au format UTF-16 ; ou un C étroit de type et vous assurer que le client l’encodage peut représenter tous les caractères de la source de données (il est toujours possible en UTF-8.)

Pour plus d’informations sur les classements et les encodages, consultez [prise en charge Unicode et du classement](../../../relational-databases/collations/collation-and-unicode-support.md).

Il existe des différences de conversion de codage entre Windows et de plusieurs versions de la bibliothèque iconv sur Linux et macOS. Données de texte dans la page de codes 1255 (hébreu) ont un seul point de code (0xCA) qui se comporte différemment lors de la conversion au format Unicode. Sous Windows, ce caractère convertit le point de code UTF-16 de 0x05BA. Sur Mac OS et Linux avec libiconv les versions antérieures à 1.15, il convertit en 0x00CA. Sur Linux avec les bibliothèques iconv qui ne prennent pas en charge la version 2003 de Big5/CP950 (nommé `BIG5-2003`), caractères ajoutés avec cette révision ne convertira pas correctement. Dans la page de codes 932 (japonais, Shift-JIS), le résultat du décodage de pas à l’origine définies dans la norme de codage de caractères diffère également. Par exemple, l’octet 0 x 80 convertit à U + 0080 sur Windows, mais peut devenir 30FB U + sur Linux et macOS, selon la version d’iconv.

Dans ODBC Driver 13 et 13.1, lorsque les caractères multi-octets UTF-8 ou des substituts UTF-16 sont réparties entre les mémoires tampons SQLPutData, il entraîne une altération des données. Utilisez des mémoires tampons pour diffuser en continu les SQLPutData qui ne se terminent pas dans des encodages de caractères partiels. Cette restriction a été supprimée avec 17 du pilote ODBC.

## <a name="additional-notes"></a>Remarques supplémentaires  

1.  Vous pouvez effectuer une connexion administrateur dédiée (DAC) à l’aide de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] l’authentification et **hôte, port**. Un membre du rôle Sysadmin doit d’abord découvrir le port DAC. Consultez [connexion de Diagnostic pour les administrateurs de base de données](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/diagnostic-connection-for-database-administrators#dac-port) pour découvrir comment. Par exemple, si le port DAC était 33000, vous pouvez connecter à l’aide de `sqlcmd` comme suit :  

    ```
    sqlcmd –U <user> -P <pwd> -S <host>,33000
    ```

    > [!NOTE]  
    > Connexions DAC doivent utiliser [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] l’authentification.  
    
2.  Le Gestionnaire de pilotes UnixODBC retourne « Identificateur d’option/d’attribut non valide » pour tous les attributs d’instruction quand ils sont passés par le biais de SQLSetConnectAttr. Sous Windows, quand SQLSetConnectAttr reçoit une valeur d’attribut instruction, elle entraîne le pilote définir cette valeur sur toutes les instructions actives qui sont des enfants du handle de connexion.  

## <a name="see-also"></a>Voir aussi  
[Forum aux Questions](../../../connect/odbc/linux-mac/frequently-asked-questions-faq-for-odbc-linux.md)

[Problèmes connus dans cette version du pilote](../../../connect/odbc/linux-mac/known-issues-in-this-version-of-the-driver.md)

[Notes de publication](../../../connect/odbc/linux-mac/release-notes.md)
