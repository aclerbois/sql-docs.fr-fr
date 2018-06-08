---
title: sp_addlinkedserver (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 09/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_addlinkedserver_TSQL
- sp_addlinkedserver
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addlinkedserver
ms.assetid: fed3adb0-4c15-4a1a-8acd-1b184aff558f
caps.latest.revision: 70
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 54fc43ecec6c26435165c9c3491bfba2f1f675ea
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddlinkedserver-transact-sql"></a>sp_addlinkedserver (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un serveur lié. Un serveur lié autorise l'accès à des sources de données OLE DB par l'intermédiaire de requêtes distribuées et hétérogènes. Une fois un serveur lié est créé à l’aide de **sp_addlinkedserver**et distribuée, les requêtes peuvent être exécutées sur ce serveur. Si le serveur lié est défini comme une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les procédures stockées distantes peuvent être exécutées.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addlinkedserver [ @server= ] 'server' [ , [ @srvproduct= ] 'product_name' ]   
     [ , [ @provider= ] 'provider_name' ]  
     [ , [ @datasrc= ] 'data_source' ]   
     [ , [ @location= ] 'location' ]   
     [ , [ @provstr= ] 'provider_string' ]   
     [ , [ @catalog= ] 'catalog' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@server=** ] **'***server***'**  
 Nom du serveur lié à créer. *server* est de type **sysname**et n'a pas de valeur par défaut.  
  
 [  **@srvproduct=** ] **'***product_name***'**  
 Nom de produit de la source de données OLE DB à ajouter comme serveur lié. *product_name* est **nvarchar (** 128 **)**, avec NULL comme valeur par défaut. Si **SQL Server**, *Nom_Fournisseur*, *data_source*, *emplacement*, *provider_string*, et *catalogue* ne doivent pas être spécifiés.  
  
 [  **@provider=** ] **'***Nom_Fournisseur***'**  
 ID de programme unique (PROGID) du fournisseur OLE DB correspondant à la source de données. *Nom_Fournisseur* doit être unique pour le fournisseur OLE DB spécifié installé sur l’ordinateur actuel. *Nom_Fournisseur* est **nvarchar (** 128 **)**, avec une valeur par défaut NULL ; Toutefois, si *Nom_Fournisseur* est omis, SQLNCLI est utilisé. (L'utilisation de SQLNCLI et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vous redirigera vers la version la plus récente du fournisseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB.) Le fournisseur OLE DB doit être inscrit dans le Registre avec le PROGID spécifié.  
  
 [  **@datasrc=** ] **'***data_source***'**  
 Nom de la source de données, tel qu'il est interprété par le fournisseur OLE DB. *data_source* est **nvarchar (** 4000 **)**. *data_source* est transmis comme propriété DBPROP_INIT_DATASOURCE pour initialiser le fournisseur OLE DB.  
  
 [  **@location=** ] **'***emplacement***'**  
 Emplacement de la base de données, tel qu'il est interprété par le fournisseur OLE DB. *emplacement* est **nvarchar (** 4000 **)**, avec NULL comme valeur par défaut. *emplacement* est transmis comme propriété DBPROP_INIT_LOCATION pour initialiser le fournisseur OLE DB.  
  
 [  **@provstr=** ] **'***provider_string***'**  
 Chaîne de connexion spécifique au fournisseur OLE DB identifiant une source de données unique. *provider_string* est **nvarchar (** 4000 **)**, avec NULL comme valeur par défaut. *l’argument provstr* est passé à IDataInitialize ou défini comme propriété DBPROP_INIT_PROVIDERSTRING pour initialiser le fournisseur OLE DB.  
  
 Lorsque le serveur lié est créé par rapport à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif, l’instance peut être spécifiée à l’aide du mot clé SERVER en tant que serveur =*nom_serveur*\\*instancename* pour spécifier une instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *nom_serveur* est le nom de l’ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d’exécution, et *instancename* est le nom de l’instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle l’utilisateur est connecté.  
  
> [!NOTE]  
>  Pour accéder à une base de données miroir, une chaîne de connexion doit contenir le nom de la base de données. Ce nom est nécessaire pour permettre au fournisseur d'accès aux données d'effectuer des tentatives de basculement. La base de données peut être spécifié dans le **@provstr** ou **@catalog** paramètre. Le cas échéant, la chaîne de connexion peut également fournir un nom de partenaire de basculement.  
  
 [  **@catalog=** ] **'***catalogue***'**  
 Catalogue à utiliser lors de l'établissement d'une connexion au fournisseur OLE DB. *catalogue* est **sysname**, avec NULL comme valeur par défaut. *catalogue* est transmis comme propriété DBPROP_INIT_CATALOG pour initialiser le fournisseur OLE DB. Lorsque le serveur lié est défini sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le catalogue fait référence à la base de données par défaut sur laquelle le serveur lié est mappé.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 Le tableau suivant présente les façons dont un serveur lié peut être configuré pour des sources de données accessibles via OLE DB. Un serveur lié peut être configuré au moyen de plusieurs méthodes pour une même source de données ; il peut y avoir plusieurs lignes pour un type de source de données. Ce tableau indique également la **sp_addlinkedserver** les valeurs de paramètre à utiliser pour la configuration du serveur lié.  
  
|Source de données OLE DB distante|Fournisseur OLE DB|product_name|provider_name|data_source|location|provider_string|catalogue|  
|-------------------------------|---------------------|-------------------|--------------------|------------------|--------------|----------------------|-------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Fournisseur OLE DB Native Client|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] <sup>1</sup> (par défaut)||||||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Fournisseur OLE DB Native Client||**SQLNCLI**|Nom réseau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (pour l'instance par défaut)|||Nom de base de données (facultatif)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Fournisseur OLE DB Native Client||**SQLNCLI**|*nom_serveur*\\*instancename* (pour une instance particulière)|||Nom de base de données (facultatif)|  
|Oracle, version 8 et ultérieure|Fournisseur Oracle pour OLE DB|Tout|**OraOLEDB.Oracle**|Alias de la base de données Oracle||||  
|Access/Jet|Fournisseur Microsoft OLE DB pour Jet|Tout|**Microsoft.Jet.OLEDB.4.0**|Nom d'accès complet du fichier de base de données Jet||||  
|Source de données ODBC|Fournisseur Microsoft OLE DB pour ODBC|Tout|**MSDASQL**|Système DSN de la source de données ODBC||||  
|Source de données ODBC|Fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour ODBC|Tout|**MSDASQL**|||Chaîne de connexion ODBC||  
|Système de fichiers|Fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour le service d'indexation|Tout|**MSIDXS**|Nom du catalogue du Service d'indexation||||  
|Feuille de calcul [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel|Fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour Jet|Tout|**Microsoft.Jet.OLEDB.4.0**|Chemin complet du fichier Excel||Excel 5.0||  
|Base de données IBM DB2|Fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour DB2|Tout|**DB2OLEDB**|||Consultez [!INCLUDE[msCoName](../../includes/msconame-md.md)] fournisseur OLE DB pour la documentation DB2.|Nom de catalogue de base de données DB2|  
  
 <sup>1</sup> cette méthode de configuration d’un serveur lié force le nom du serveur lié pour être le même que le nom de réseau de l’instance distante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez *data_source* pour spécifier le serveur.  
  
 <sup>2</sup> « Tout » indique que le nom du produit peut être quoi que ce soit.  
  
 Le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif est le fournisseur qui est utilisé avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si aucun nom de fournisseur n’est spécifié ou si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est spécifié comme nom de produit. Même si vous spécifiez l'ancien nom du fournisseur, SQLOLEDB, il est remplacé par SQLNCLI lorsqu'il est persistant pour le catalogue.  
  
 Le *data_source*, *emplacement*, *provider_string*, et *catalogue* paramètres identifient la base de données ou les bases de données du serveur lié pointe vers. Si un de ces paramètres a la valeur NULL, la propriété d'initialisation OLE DB correspondante n'est pas définie.  
  
 Dans un environnement ordonné en clusters, lorsque vous spécifiez des noms de fichiers qui pointent vers des sources de données OLE DB, utilisez le nom UNC (Universal Naming Convention) ou un lecteur partagé pour spécifier l'emplacement.  
  
 **sp_addlinkedserver** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
> [!IMPORTANT]  
>  Quand un serveur lié est créé à l’aide de **sp_addlinkedserver**, un auto-mappage par défaut est ajouté pour toutes les connexions locales. Pour les fournisseurs non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentifiées peuvent accéder au fournisseur sous le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les administrateurs doivent envisager d'utiliser `sp_droplinkedsrvlogin <linkedserver_name>, NULL` pour supprimer le mappage global.  
  
## <a name="permissions"></a>Autorisations  
 Le `sp_addlinkedserver` instruction nécessite la `ALTER ANY LINKED SERVER` autorisation. (SSMS **nouveau serveur lié** boîte de dialogue est implémentée de manière qui requiert l’appartenance dans le `sysadmin` rôle serveur fixe.)  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-microsoft-sql-server-native-client-ole-db-provider"></a>A. Utilisation du fournisseur Microsoft SQL Server Native Client OLE DB  
 L'exemple suivant crée un serveur lié nommé `SEATTLESales`. Le nom du produit est `SQL Server` ; aucun nom de fournisseur n'est utilisé.  
  
```  
USE master;  
GO  
EXEC sp_addlinkedserver   
   N'SEATTLESales',  
   N'SQL Server';  
GO  
```  
  
 L’exemple suivant crée un serveur lié `S1_instance1` sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif.  
  
```  
EXEC sp_addlinkedserver     
   @server=N'S1_instance1',   
   @srvproduct=N'',  
   @provider=N'SQLNCLI',   
   @datasrc=N'S1\instance1';  
```  
  
### <a name="b-using-the-microsoft-ole-db-provider-for-microsoft-access"></a>B. Utilisation du fournisseur Microsoft OLE DB pour Microsoft Access  
 Le fournisseur Microsoft.Jet.OLEDB.4.0 se connecte aux bases de données Microsoft Access qui utilisent le format 2002-2003. L'exemple suivant crée un serveur lié nommé `SEATTLE Mktg`.  
  
> [!NOTE]  
>  Cet exemple suppose que [!INCLUDE[msCoName](../../includes/msconame-md.md)] accès et l’exemple **Northwind** base de données sont installés et que le **Northwind** base de données se trouve dans C:\Msoffice\Access\Samples.  
  
```  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Mktg',   
   @provider = N'Microsoft.Jet.OLEDB.4.0',   
   @srvproduct = N'OLE DB Provider for Jet',  
   @datasrc = N'C:\MSOffice\Access\Samples\Northwind.mdb';  
GO  
```  
  
 Le fournisseur Microsoft.ACE.OLEDB.12.0 se connecte aux bases de données Microsoft Access qui utilisent le format 2007. L'exemple suivant crée un serveur lié nommé `SEATTLE Mktg`.  
  
> [!NOTE]  
>  Cet exemple suppose que [!INCLUDE[msCoName](../../includes/msconame-md.md)] accès et l’exemple **Northwind** base de données sont installés et que le **Northwind** base de données se trouve dans C:\Msoffice\Access\Samples.  
  
```  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Mktg',   
   @provider = N'Microsoft.ACE.OLEDB.12.0',   
   @srvproduct = N'OLE DB Provider for ACE',  
   @datasrc = N'C:\MSOffice\Access\Samples\Northwind.accdb';  
GO  
```  
  
### <a name="c-using-the-microsoft-ole-db-provider-for-odbc-with-the-datasource-parameter"></a>C. Utilisation du fournisseur Microsoft OLE DB pour ODBC avec le paramètre data_source  
 L’exemple suivant crée un serveur lié nommé `SEATTLE Payroll` qui utilise le [!INCLUDE[msCoName](../../includes/msconame-md.md)] fournisseur OLE DB pour ODBC (`MSDASQL`) et le *data_source* paramètre.  
  
> [!NOTE]  
>  Le nom de la source de données ODBC spécifiée doit être défini comme DSN système dans le serveur avant d'utiliser le serveur lié.  
  
```  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Payroll',   
   @srvproduct = N'',  
   @provider = N'MSDASQL',   
   @datasrc = N'LocalServer';  
GO  
```  
  
### <a name="d-using-the-microsoft-ole-db-provider-for-excel-spreadsheet"></a>D. Utilisation du fournisseur Microsoft OLE DB pour une feuille de calcul Excel  
 Pour créer une définition de serveur lié à l’aide de le [!INCLUDE[msCoName](../../includes/msconame-md.md)] fournisseur OLE DB pour Jet accéder à une feuille de calcul Excel au format 1997-2003, tout d’abord créer une plage nommée dans Excel en spécifiant les colonnes et les lignes de la feuille de calcul Excel à sélectionner. Le nom de la plage peut correspondre à un nom de table dans une requête distribuée.  
  
```  
EXEC sp_addlinkedserver 'ExcelSource',  
   'Jet 4.0',  
   'Microsoft.Jet.OLEDB.4.0',  
   'c:\MyData\DistExcl.xls',  
   NULL,  
   'Excel 5.0';  
GO  
```  
  
 Pour accéder aux données d'une feuille de calcul Excel, associez une plage de cellules à un nom. La requête suivante peut s'utiliser pour accéder à la plage nommée `SalesData` en tant que table en utilisant le serveur lié défini précédemment.  
  
```  
SELECT *  
   FROM ExcelSource...SalesData;  
GO  
```  
  
 Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute sous un compte de domaine qui peut accéder à un partage distant, il est possible d'utiliser un chemin UNC à la place d'un lecteur mappé.  
  
```  
EXEC sp_addlinkedserver 'ExcelShare',  
   'Jet 4.0',  
   'Microsoft.Jet.OLEDB.4.0',  
   '\\MyServer\MyShare\Spreadsheets\DistExcl.xls',  
   NULL,  
   'Excel 5.0';  
```  
  
 Pour vous connecter à une feuille de calcul Excel au format Excel 2007, utilisez le fournisseur ACE.  
  
```  
EXEC sp_addlinkedserver @server = N'ExcelDataSource',   
@srvproduct=N'ExcelData', @provider=N'Microsoft.ACE.OLEDB.12.0',   
@datasrc=N'C:\DataFolder\People.xlsx',  
@provstr=N'EXCEL 12.0' ;  
  
```  
  
### <a name="e-using-the-microsoft-ole-db-provider-for-jet-to-access-a-text-file"></a>E. Utilisation du fournisseur Microsoft OLE DB pour Jet pour accéder à un fichier texte  
 Le code exemple suivant crée un serveur lié pour accéder directement aux fichiers texte, sans lier les fichiers en tant que tables dans un fichier .mdb de Microsoft Access. Le fournisseur est `Microsoft.Jet.OLEDB.4.0` ; la chaîne de caractères du fournisseur est `Text`.  
  
 La source de données est le chemin d'accès complet au répertoire qui contient les fichiers texte. Un fichier schema.ini, qui décrit la structure des fichiers texte, doit se trouver dans le même répertoire que les fichiers texte. Pour plus d'informations sur la création d'un fichier Schema.ini, consultez la documentation du moteur de base de données Jet.  
  
```  
--Create a linked server.  
EXEC sp_addlinkedserver txtsrv, N'Jet 4.0',   
   N'Microsoft.Jet.OLEDB.4.0',  
   N'c:\data\distqry',  
   NULL,  
   N'Text';  
GO  
  
--Set up login mappings.  
EXEC sp_addlinkedsrvlogin txtsrv, FALSE, Admin, NULL;  
GO  
  
--List the tables in the linked server.  
EXEC sp_tables_ex txtsrv;  
GO  
  
--Query one of the tables: file1#txt  
--using a four-part name.   
SELECT *   
FROM txtsrv...[file1#txt];  
```  
  
### <a name="f-using-the-microsoft-ole-db-provider-for-db2"></a>F. Utilisation du fournisseur Microsoft OLE DB pour DB2  
 Le code exemple suivant crée un serveur lié nommé `DB2` qui utilise le `Microsoft OLE DB Provider for DB2`.  
  
```  
EXEC sp_addlinkedserver  
   @server=N'DB2',  
   @srvproduct=N'Microsoft OLE DB Provider for DB2',  
   @catalog=N'DB2',  
   @provider=N'DB2OLEDB',  
   @provstr=N'Initial Catalog=PUBS;  
       Data Source=DB2;  
       HostCCSID=1252;  
       Network Address=XYZ;  
       Network Port=50000;  
       Package Collection=admin;  
       Default Schema=admin;';  
```  
  
### <a name="g-add-a-includesssdsfullincludessssdsfull-mdmd-as-a-linked-server-for-use-with-distributed-queries-on-cloud-and-on-premise-databases"></a>G. Ajouter [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] en tant que serveur lié pour une utilisation avec des requêtes distribuées dans les bases de données du cloud et locales  
 Ajoutez [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] en tant que serveur lié, puis utilisez-le avec des requêtes distribuées qui couvrent les bases de données du cloud et locales. Il s'agit d'un composant pour les solutions hybrides de base de données couvrant les réseaux d'entreprise locaux et le cloud Windows Azure.  
  
 La boîte du produit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient la fonctionnalité de requête distribuée, qui vous permet d'écrire des requêtes afin d'associer des données de sources locales et des données de sources distantes (y compris des données de sources de données autres que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) définies en tant que serveurs liés. Chaque [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (sauf le maître virtuel) peut être ajoutée en tant que serveur lié individuel, puis être utilisée directement dans vos applications de base de données comme toute autre base de données.  
  
 Les avantages de l'utilisation de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] sont la simplicité de gestion, la haute disponibilité, l'extensibilité, l'utilisation d'un modèle de développement familier et un modèle de données relationnelles. La configuration de votre application de base de données détermine le mode d'utilisation de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] dans le cloud. Déplacez toutes les données à la fois vers [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], ou déplacez progressivement certaines de vos données et conservez les autres localement. Pour cette application de base de données hybride, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] peut maintenant être ajoutée en tant que serveurs liés et l'application de base de données peut publier des requêtes distribuées afin d'associer des données de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et de sources de données locales.  
  
 Voici un exemple simple expliquant comment se connecter à une [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] à l'aide de requêtes distribuées :  
  
```  
------ Configure the linked server  
-- Add one Windows Azure SQL DB as Linked Server  
EXEC sp_addlinkedserver  
@server='myLinkedServer', -- here you can specify the name of the linked server  
@srvproduct='',       
@provider='sqlncli', -- using SQL Server Native Client  
@datasrc='myServer.database.windows.net',   -- add here your server name  
@location='',  
@provstr='',  
@catalog='myDatabase'  -- add here your database name as initial catalog (you cannot connect to the master database)  
-- Add credentials and options to this linked server  
EXEC sp_addlinkedsrvlogin  
@rmtsrvname = 'myLinkedServer',  
@useself = 'false',  
@rmtuser = 'myLogin',             -- add here your login on Azure DB  
@rmtpassword = 'myPassword' -- add here your password on Azure DB  
EXEC sp_serveroption 'myLinkedServer', 'rpc out', true;  
------ Now you can use the linked server to execute 4-part queries  
-- You can create a new table in the Azure DB  
exec ('CREATE TABLE t1tutut2(col1 int not null CONSTRAINT PK_col1 PRIMARY KEY CLUSTERED (col1) )') at myLinkedServer  
-- Insert data from your local SQL Server  
exec ('INSERT INTO t1tutut2 VALUES(1),(2),(3)') at myLinkedServer  
  
-- Query the data using 4-part names  
select * from myLinkedServer.myDatabase.dbo.myTable  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Distributed des procédures stockées de requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_dropserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [sp_setnetname &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setnetname-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
