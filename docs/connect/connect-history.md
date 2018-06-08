---
title: Historique de pilote pour Microsoft SQL Server | Documents Microsoft
ms.custom: ''
ms.date: 5/4/2018
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: David-Engel
ms.author: v-daveng
manager: kenvh
ms.openlocfilehash: 2f2f4bafe48018da5c6f634b272a540021fd4ca2
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="driver-history-for-microsoft-sql-server"></a>Historique de pilote pour Microsoft SQL Server

Cette page décrit les technologies de connexion de données d’historique de Microsoft pour se connecter à SQL Server.

## <a name="odbc"></a>ODBC

Il existe trois générations distinctes des pilotes ODBC de Microsoft pour SQL Server. Le premier pilote ODBC de « SQL Server » est toujours fourni dans le cadre de [Windows Data Access Components](#microsoft-or-windows-data-access-components). Il n’est pas recommandé d’utiliser de ce pilote pour tout nouveau développement. À compter de SQL Server 2005, le [SQL Server Native Client](#sql-server-native-client) inclut une interface ODBC et le pilote ODBC fourni avec SQL Server 2005 via SQL Server 2012. Il n’est pas recommandé d’utiliser de ce pilote pour tout nouveau développement. Une fois SQL Server 2012, le [Microsoft ODBC Driver for SQL Server](#microsoft-odbc-driver-for-sql-server) le pilote est mis à jour avec les fonctionnalités du serveur les plus récentes à l’avenir.

### <a name="sql-server-native-client"></a>SQL Server Native Client

SQL Server Native Client est une bibliothèque autonome qui est utilisée pour OLE DB et ODBC. SQL Server Native Client (SNAC souvent abrégé) a été inclus dans SQL Server 2005 à l’année 2012. SQL Server Native Client peut être utilisé pour les applications qui doivent tirer parti des nouvelles fonctionnalités introduites dans SQL Server 2005 via SQL Server 2012. (Microsoft/Windows Data Access Components ne sont pas mis à jour pour ces nouvelles fonctionnalités dans SQL Server.) Pour les nouvelles fonctionnalités au-delà de SQL Server 2012, SQL Server Native Client ne sera pas modifié. Basculer vers le pilote Microsoft ODBC pour SQL Server ou le pilote Microsoft OLE DB pour SQL Server si vous souhaitez tirer parti des nouvelles fonctionnalités de SQL Server à l’avenir.

Pour obtenir une documentation complète de SQL Server Native Client, consultez le [documentation de SQL Server Native Client](../relational-databases/native-client/sql-server-native-client-programming.md).

### <a name="microsoft-odbc-driver-for-sql-server"></a>Pilote Microsoft ODBC pour SQL Server

Une fois SQL Server 2012, principal ODBC driver for SQL Server a été développé et publié en tant que le pilote Microsoft ODBC pour SQL Server. Pour plus d’informations, consultez la [le pilote Microsoft ODBC pour la documentation de SQL Server](./odbc/microsoft-odbc-driver-for-sql-server.md).

## <a name="ole-db"></a>OLE DB

Il existe trois générations distinctes des fournisseurs OLE DB Microsoft pour SQL Server. Le premier « Microsoft OLE DB Provider for SQL Server » (SQLOLEDB) sont toujours fournis dans le cadre de [Windows Data Access Components](#microsoft-or-windows-data-access-components). Ce fournisseur ne sera pas mis à jour avec les nouvelles fonctionnalités et il n’est pas recommandé d’utiliser de ce pilote pour tout nouveau développement. À compter de SQL Server 2005, le [SQL Server Native Client](#sql-server-native-client) inclut une interface de fournisseur OLE DB (SQLNCLI) et est le fournisseur OLE DB fournis avec SQL Server 2005 via SQL Server 2017. Il a été [annoncées comme déconseillées dans 2011](https://blogs.msdn.microsoft.com/sqlnativeclient/2011/08/29/microsoft-is-aligning-with-odbc-for-native-relational-data-access/) et il n’est pas recommandé d’utiliser de ce pilote pour tout nouveau développement. En 2017, technologie d’accès aux données OLE DB a été par la suite [undeprecated et une nouvelle version planifiée a été annoncée](https://blogs.msdn.microsoft.com/sqlnativeclient/2017/10/06/announcing-the-new-release-of-ole-db-driver-for-sql-server/) pour 2018. Le nouveau fournisseur OLE DB est appelé le « Microsoft OLE DB pilote pour SQL Server » (MSOLEDBSQL) et actuellement géré et pris en charge.

## <a name="adonet"></a>ADO.NET

ADO.NET est une nouveauté de Microsoft .NET Framework et continue d’être améliorées et conservés. Il est un composant majeur de Microsoft .NET Framework. Pour plus d’informations, consultez [Microsoft ADO.NET pour SQL Server](ado-net/microsoft-ado-net-for-sql-server.md).

## <a name="jdbc"></a>JDBC

### <a name="microsoft-jdbc-driver-for-sql-server"></a>Pilote Microsoft JDBC pour SQL Server

Introduite dans 2000, le pilote JDBC Microsoft pour SQL Server continue améliorée et la maintenance. Il a été open source dans 2016. Pour plus d’informations, notamment la façon de télécharger le pilote, consultez [vue d’ensemble du pilote JDBC](./jdbc/overview-of-the-jdbc-driver.md).

## <a name="php"></a>PHP

### <a name="microsoft-drivers-for-php-for-sql-server"></a>Pilotes Microsoft SQL Server pour PHP

Le Microsoft Drivers for PHP for SQL Server introduit dans 2009 sous forme de projet open source, continuer améliorée et la maintenance. Pour plus d’informations, notamment la façon de télécharger le pilote PHP, consultez [Microsoft Drivers for PHP for SQL Server](./php/microsoft-php-driver-for-sql-server.md).

## <a name="nodejs"></a>Node.js

### <a name="microsoft-driver-for-nodejs-for-sql-server"></a>Pilote Microsoft pour Node.js pour SQL Server

Microsoft Driver pour Node.js pour SQL Server permet aux applications de la Node.js sur Microsoft Windows et Microsoft Windows Azure pour accéder à Microsoft SQL Server et Microsoft Windows Azure SQL Database. Les efforts de développement sont n’est plus considérés ce pilote. Il n’est pas recommandé pour créer des applications à l’aide de Microsoft Driver pour Node.js pour SQL Server.

Pour plus d’informations sur Microsoft Driver pour Node.js pour SQL Server, consultez [WindowsAzure / nœud-sqlserver](https://github.com/Azure/node-sqlserver).

### <a name="tedious"></a>Fastidieux

Microsoft contribue à actuellement et prend en charge le module fastidieux open source dans Node.js pour la connectivité à SQL Server à l’aide de JavaScript. Pour plus d’informations, consultez [pilote Node.js pour SQL Server](./node-js/node-js-driver-for-sql-server.md).

## <a name="microsoft-or-windows-data-access-components"></a>Windows Data Access Components ou Microsoft

Microsoft/Windows Data Access Components (MDAC/WDAC) sont livrés avec prise en charge par Windows pour l’application vers l’arrière compatibilité et ne font pas partie de la pile de technologies SQL Server en cours. Aucune nouvelle fonctionnalité ne va être ajoutée pour les composants dans MDAC/WDAC et il n’est pas recommandé de les utiliser pour un nouveau développement d’applications.

Dans le cadre de ce document, vous pouvez diviser la pile MDAC/WDAC dans les composants suivants, selon les produits et technologies :

* **ADO** (y compris les ADOMD et ADOX)
* **OLE DB** (y compris les Services principaux OLE DB, fournisseur OLE DB SQL Server, fournisseur Oracle OLE DB, le fournisseur OLE DB pour pilotes ODBC, fournisseur de données de forme et à distance du fournisseur de données)
* **ODBC** (y compris le Gestionnaire de pilotes ODBC, le pilote ODBC SQL et pilote ODBC pour Oracle)

### <a name="mdacwdac-components"></a>Composants MDAC/WDAC

MDAC/WDAC comprend les composants suivants :

* **ODBC :** l’interface de Microsoft ODBC Open Database Connectivity () est une interface de programmation en langage C qui permet aux applications d’accéder aux données à partir de divers systèmes de gestion de base de données (SGBD). Les applications qui utilisent cette API sont limitées à l’accès aux sources de données relationnelles uniquement.
* **OLE DB :** OLE DB est un ensemble d’interfaces COM pour l’accès aux données dans une variété de banques de données. Fournisseurs OLE DB existent pour accéder aux données dans les bases de données, les systèmes de fichiers, les banques de messages, services d’annuaire, flux de travail et banques de documents.
* **ADO :** ActiveX Data Objects (ADO) fournit un modèle de programmation de haut niveau. Bien qu’un peu moins performantes que le codage directement à OLE DB ou ODBC, ADO est simple apprendre à utiliser. Il peut être utilisé à partir de langages de script, tel que Microsoft Visual Basic Scripting Edition (VBScript) ou Microsoft JScript.
* **ADOMD :** multidimensionnelle ADO (ADOMD) doit être utilisé avec les fournisseurs de données multidimensionnelles tel que Microsoft OLAP fournisseur, également appelé Microsoft Analysis Services Provider. Aucun principales améliorations fonctionnelles n’apportées à ce dernier depuis MDAC 2.0.
* **ADOX :** Extensions ADO pour DDL et sécurité (ADOX) activer la création et la modification des définitions d’une base de données, une table, un index ou une procédure stockée. Vous pouvez utiliser ADOX avec n’importe quel fournisseur. Le fournisseur Microsoft Jet OLE DB fournit une prise en charge complète pour ADOX, alors que le fournisseur OLE DB Microsoft SQL Server fournit la prise en charge limitée.
* **Bibliothèques de réseau Microsoft SQL Server :** les bibliothèques réseau du serveur SQL autoriser SQLOLEDB et SQLODBC communiquer avec la base de données SQL Server. Les bibliothèques réseau de serveur SQL suivantes ont été déconseillées dans les versions MDAC/WDAC : Banyan Vines, AppleTalk, ServerNet, IPX/SPX, Giganet et RPC. TCP/IP et canaux nommés continueront à être pris en charge et sont disponibles sur le système d’exploitation de Windows 64 bits.
* **MSDASQL :** le fournisseur Microsoft OLE DB pour ODBC (MSDASQL) permet aux applications qui reposent sur OLE DB et ADO (qui utilise OLEDB en interne) pour accéder aux sources de données via un pilote ODBC. MSDASQL est un fournisseur OLE DB qui se connecte à ODBC, au lieu d’une base de données. Il est destiné comme un pont de OLE DB à un pilote ODBC lorsqu’aucun fournisseur OLE DB directe n’existe pour une source de données. MSDASQL fourni avec le système d’exploitation Windows et Windows Server 2008 et Vista SP1 ont été que mises à jour de la première Windows pour inclure une version 64 bits de la technologie.

### <a name="deprecated-mdacwdac-components"></a>Composants MDAC/WDAC déconseillés

Ces composants sont toujours pris en charge dans la version actuelle de MDAC/WDAC, mais ils peuvent être supprimés dans les versions futures. Lorsque vous développez des applications, Microsoft vous recommande d’éviter d’utiliser ces composants. En outre, lorsque vous mettez à niveau ou modifiez des applications existantes, supprimez toute dépendance vis-à-vis de ces composants.

* **SQLOLEDB :** le fournisseur Microsoft OLE DB pour SQL Server (SQLOLEDB), qui prend en charge l’accès à Microsoft SQL Server, a été déconseillé. Sa connectivité vers les versions ultérieures de SQL Server ne peut pas être pris en charge. Capacité de se connecter aux versions antérieures à SQL Server 7 sera supprimé du système d’exploitation après Windows 7. Nouvelles applications doivent utiliser le pilote Microsoft OLE DB pour SQL Server (MSOLEDBSQL), qui prend en charge les nouvelles fonctionnalités de SQL Server. Les applications existantes la migration vers le pilote Microsoft OLE DB pour SQL Server et pour de meilleures performances, la fiabilité et prise en charge. Pour plus d’informations, consultez [mise à jour d’une Application de pilote OLE DB pour SQL Server à partir de MDAC](oledb/applications/updating-an-application-to-oledb-driver-for-sql-server-from-mdac.md).
* **SQLODBC :** le Microsoft SQL Server ODBC Driver (SQLODBC), qui prend en charge l’accès à Microsoft SQL Server, a été déconseillée. Sa connectivité vers les versions ultérieures de SQL Server ne peut pas être pris en charge. Capacité de se connecter aux versions antérieures à SQL Server 7 sera supprimé du système d’exploitation après Windows 7. Nouvelles applications doivent utiliser le pilote Microsoft ODBC pour SQL Server sur Windows, qui prend en charge les nouvelles fonctionnalités de SQL Server. Les applications existantes la migration vers le pilote Microsoft ODBC pour SQL Server ainsi de meilleures performances, de fiabilité et de prise en charge. Pour plus d’informations pertinentes, consultez [mise à jour d’une Application vers SQL Server Native Client à partir de MDAC](../relational-databases/native-client/applications/updating-an-application-to-sql-server-native-client-from-mdac.md).
* **Moteur de base de données Microsoft Jet 4.0 :** depuis la version 2.6, MDAC ne contient plus de composants Jet. En d’autres termes, MDAC 2.6, 2.7 et 2.8 ne contiennent pas Microsoft Jet, le fournisseur Microsoft Jet OLE DB, les pilotes de base de données ODBC Desktop ou Jet objets DAO (Data Access). Les composants 4.0 de moteur de base de données Microsoft Jet est entré dans un état de désapprobation fonctionnel subi l’ingénierie et améliorations au niveau des fonctionnalités n’ont pas reçus depuis une partie de Microsoft Windows dans Windows 2000.

  Aucune version 64 bits du moteur de base de données Jet, le pilote OLE DB de Jet, les pilotes Jet ODBC ou DAO de Jet n’est disponible. Pour plus d’informations, consultez [l’article 957570](http://support.microsoft.com/kb/957570). Sur les versions 64 bits de Windows, Jet de 32 bits s’exécute sous le sous-système WOW64 de Windows. Pour plus d’informations sur WOW64, consultez la [MSDN WOW64 documentation](https://msdn.microsoft.com/library/windows/desktop/aa384274(v=vs.85).aspx). Les applications 64 bits natives ne peuvent pas communiquer avec les pilotes Jet 32 bits s’exécutant dans WOW64.

  Au lieu de Microsoft Jet, Microsoft recommande l’utilisation de [Microsoft SQL Server Express Edition](https://www.microsoft.com/sql-server/sql-server-editions-express) lors du développement de nouveau, les applications de Microsoft Access qui requièrent une banque de données relationnelle. Ces applications Jet nouveau ou converties peuvent continuer à utiliser Jet comme à l’aide de Microsoft Office 2003 et les fichiers antérieurs (.mdb et .xls) pour le stockage de données non primaire. Toutefois, pour ces applications, vous devez planifier migrer à partir de Jet au pilote système Office 2007. Vous pouvez [télécharger le pilote d’Office System 2007](https://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=7554f536-8c28-4598-9b72-ef94e038c891), qui vous permet de lire et écrire dans les fichiers existants dans Office 2003 (.mdb et .xls) ou les formats de fichier Office 2007 (*.accdb, *.xlsm, *.xlsx et *.xlsb).

  > [!IMPORTANT]
  > Lisez le contrat de licence utilisateur final de 2007 Office System pour connaître les limitations d’utilisation spécifiques.

  > [!NOTE]
  > Applications SQL Server peuvent également accéder à Office System 2007 et versions antérieures, les fichiers à partir de la connectivité de données hétérogènes de SQL Server et les Services d’intégration ainsi, via le pilote d’Office System 2007. En outre, des applications 64 bits SQL Server peuvent accéder à Jet de 32 bits et les fichiers d’Office System 2007 à l’aide de 32 bits SQL Server Integration Services (SSIS) sur Windows 64 bits.

* **MSDADS :** avec le fournisseur Microsoft OLE DB pour les données de mise en forme (MSDADS), vous pouvez créer des relations hiérarchiques entre les clés, des champs ou des ensembles de lignes dans une application. Aucune amélioration de la fonctionnalité principale n’ont été apportées depuis MDAC 2.1. Ce fournisseur a été déconseillé. Microsoft recommande d’utiliser XML, au lieu de MSDADS.
* **Oracle, ODBC et OLE DB pour Oracle :** le pilote Microsoft Oracle ODBC (ODBC Oracle) et le fournisseur Microsoft OLE DB pour Oracle (Oracle OLE DB) fournissent l’accès aux serveurs de base de données Oracle. Ils sont générés en utilisant l’Interface OCI (Oracle Call) version 7 et fournissent une prise en charge complète pour Oracle 7. En outre, il utilise une émulation de Oracle 7 pour fournir la prise en charge limitée des bases de données Oracle 8. Oracle ne prend plus en les applications qui utilisent des appels de la version 7 OCI. Ces technologies sont déconseillés. Si vous utilisez des sources de données Oracle, vous devez migrer vers le fournisseur et le pilote Oracle.
* **Services Bureau à distance :** Services de données à distance (RDS) est un mécanisme propriétaire de Microsoft pour accéder à des objets ADO Recordset à distance via Internet ou un Intranet. Services Bureau à distance est déconseillé ; Aucun principales améliorations fonctionnelles n’apportées à des services Bureau à distance depuis MDAC 2.1. Microsoft a publié le .NET Framework, qui possède des fonctionnalités étendues de SOAP et remplace les composants des services Bureau à distance. Tous les composants de serveur Services Bureau à distance seront supprimés du système d’exploitation après Windows 7.
* **JRO :** objets de réplication Jet (JRO) est déconseillée. JRO est utilisé dans ADO avec Jet (*.mdb) bases de données à créer et de compresser les bases de données Jet (.mdb) et d’effectuer la gestion de la réplication Jet. MDAC 2.7 sera sa dernière version. JRO ne sera pas disponible sur le système d’exploitation de Windows 64 bits. JRO n’est pas pris en charge dans le format de fichier Microsoft Access 2007 (*.accdb).
* **prise en charge de 16 bits ODBC :** si vous utilisez des applications 16 bits, vous devez migrer vers une application 32 bits. fonctionnalités de 16 bits sont déconseillée et sont en cours de suppression à partir de systèmes d’exploitation 64 bits. Pour plus d’informations, consultez [l’article de la base de connaissances 896458](http://support.microsoft.com/kb/896458).
* **Fournisseur OLE DB Simple (MSDAOSP) :** fournisseur Simple OLEDB offre une infrastructure permettant de créer rapidement des fournisseurs OLE DB de données simples. MSDAOSP est déconseillée.
* **Bibliothèque de curseurs ODBC :** bibliothèque de curseurs ODBC (ODBCCR32.dll) fournit les curseurs des données côté client limitée. Bibliothèque de curseurs ODBC est déconseillée ; votre application peut utiliser des implémentations de curseur côté serveur en remplacement.
* **La communication à distance Interface Out-of-Process OLE DB :** la communication à distance de OLEDB Interface (msdaps.dll) a eu une tentative pour permettre aux fournisseurs OLE DB exécuter en dehors du processus. La communication à distance OLEDB Out-of-Process Interface est déconseillée.
* **AppleTalk et des bibliothèques de réseau Banyan Vines SQL :** les bibliothèques réseau le Banyan Vines, AppleTalk, ServerNet, IPX/SPX, Giganet et RPC SQL sont déconseillées. Si vous utilisez l’une de ces technologies, vous devez modifier vos applications pour utiliser une des autres bibliothèques réseau, tels que TCP/IP et canaux nommés.

### <a name="mdacwdac-releases"></a>Versions MDAC/WDAC

Voici une liste des scénarios de prise en charge des versions antérieures de MDAC/WDAC, en commençant par la première.

* **MDAC 1.5, MDAC 2.0 et 2.1 de MDAC :** ces versions de MDAC ont été mises en production indépendants qui ont été publiées par Microsoft Windows NT Option Pack, le Kit de développement Windows ou le site Web de MDAC. Ces versions de MDAC ne sont plus prises en charge.
* **MDAC 2.5 :** cette version de MDAC a été incluse avec le système d’exploitation Windows 2000. Service packs de MDAC 2.5 étaient inclus avec les service packs Windows 2000 correspondants.
* **MDAC 2.6 :** RTM de MDAC 2.6, SP1 et SP2 étaient inclus avec Microsoft SQL Server 2000 RTM, SP1 et SP2, respectivement. En outre, ces MDAC service packs ont été publiés sur le site Web de MDAC conformément à la planification de la version du service pack Microsoft SQL Server 2000. Vous pouvez installer cette version de MDAC et ses service packs sur les plateformes Windows 2000, Windows Millennium Edition, Windows NT, Windows 95 et Windows 98. Cette version de MDAC n’est plus est prise en charge.
* **MDAC 2.7 :** cette version de MDAC était incluse avec les systèmes d’exploitation Microsoft Windows XP RTM et SP1. Vous pouvez installer cette version de MDAC et ses service packs sur les plateformes Windows 2000, Windows Millennium, Windows NT et Windows 98. Vous pouvez installer cette version sur la plateforme Windows XP uniquement via le système d’exploitation ou de ses packs de services. Cette version de MDAC n’est plus est prise en charge.
* **MDAC 2.8 :** cette version de MDAC a été inclus avec Windows Server 2003 et Windows XP SP2 et versions ultérieures. Vous pouvez également installer cette version de MDAC et ses service packs sur Windows 2000.

  * La version 32 bits de MDAC 2.8 également publiée sur le site Web de MDAC en même temps que Windows Server 2003 a été publié au client.
  * La version 64 bits de MDAC 2.8 a été publiée avec la version 64 bits de Windows Server 2003 et Windows XP.

* **Windows Data Access Components (WDAC) :** MDAC changé son nom en WDAC - « Windows Data Access Components » en commençant par Windows Vista et Windows Server 2008. WDAC est inclus dans le système d’exploitation et n’est pas disponible pour la redistribution séparément. Facilité de gestion pour WDAC est soumis au cycle de vie du système d’exploitation.

  les versions 32 bits et 64 bits de WDAC sont publiées avec les versions 32 bits et 64 bits des systèmes d’exploitation Windows, respectivement.

## <a name="obsolete-data-access-technologies"></a>Technologies d’accès aux données obsolètes

Technologies obsolètes sont des technologies pas améliorés ou mis à jour dans plusieurs versions du produit et les versions ultérieures du produit qui ne sont. N’utilisez pas ces technologies lorsque vous écrivez des nouvelles applications. Lorsque vous modifiez des applications existantes qui sont écrits à l’aide de ces technologies, envisagez de migrer ces applications ADO.NET ou une autre technologie actuelle.

Les composants suivants sont considérés comme obsolètes :

* **DB-Library :** DB-Library est un modèle de programmation spécifiques à SQL Server qui inclut des API C. Aucune amélioration de fonctionnalité à la bibliothèque DB-Library n’ont été depuis SQL Server 6.5. Sa version finale était avec SQL Server 2000, et il ne sera pas déplacée au système d’exploitation Windows 64 bits.
* **Embedded SQL (E-SQL) :** E-SQL est un modèle de programmation spécifiques à SQL Server qui permet des instructions Transact-SQL à incorporer dans le code Visual C#. Aucun améliorations n’apportées à l’E-SQL depuis SQL Server 6.5. Sa version finale était avec SQL Server 2000, et il ne sera pas déplacée au système d’exploitation Windows 64 bits.
* **Objets d’accès aux données (DAO) :** DAO fournit l’accès aux bases de données JET (Access). Cette API peut être utilisée à partir de Microsoft Visual Basic, Microsoft Visual C++ et les langages de script. Il était inclus dans Microsoft Office 2000 et Office XP. DAO 3.6 est la version finale de cette technologie. Il ne sera pas disponible sur le système d’exploitation de Windows 64 bits.
* **Objets de données à distance (RDO) :** RDO a été conçu spécifiquement pour accéder aux sources de données relationnelles ODBC à distance et facilitent l’utilisation de ODBC sans code de l’application complexe. Il a été inclus dans les versions de Microsoft Visual Basic 4, 5 et 6. RDO version 2.0 était la version finale de cette technologie.