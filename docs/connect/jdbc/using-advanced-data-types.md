---
title: À l’aide des Types de données avancés | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b39461d3-48d6-4048-8300-1a886c00756d
caps.latest.revision: 58
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4bd8d2b92c6d998c10239f3502328b016de42045
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-advanced-data-types"></a>Utilisation des types de données avancés
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] utilise les types de données avancés JDBC pour convertir le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données dans un format compréhensible par le Java en langage de programmation.  
  
## <a name="remarks"></a>Notes  
 Le tableau suivant répertorie les mappages par défaut entre avancées [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], JDBC et les types de données de langage de programmation Java.  
  
|Types SQL Server|Types JDBC (java.sql.Types)|Types langage Java|  
|----------------------|-----------------------------------|-------------------------|  
|varbinary(max)<br /><br /> image|LONGVARBINARY|Byte [] \(par défaut), Blob, InputStream, String|  
|texte<br /><br /> varchar(max)|LONGVARCHAR|String (par défaut), Clob, InputStream|  
|ntext<br /><br /> nvarchar(max)|LONGVARCHAR<br /><br /> LONGNVARCHAR (Java SE 6.0)|String (par défaut), Clob, NClob (Java SE 6.0)|  
|xml|LONGVARCHAR<br /><br /> SQLXML (Java SE 6.0)|String (par défaut), InputStream, Clob, byte[],Blob, SQLXML (Java SE 6.0)|  
|UDT<sup>1</sup>|VARBINARY|String (par défaut), byte[], InputStream|  
  
 <sup>1</sup> le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] gère l’envoi et la récupération d’UDT CLR en tant que données binaires, mais ne prend pas en charge la manipulation des métadonnées CLR.  
  
 Les sections suivantes proposent des exemples d'utilisation du pilote JDBC et des types de données avancés.  
  
## <a name="blob-and-clob-and-nclob-data-types"></a>Types de données BLOB, CLOB et NCLOB  
 Le pilote JDBC implémente toutes les méthodes des interfaces java.sql.Blob, java.sql.Clob et java.sql.NClob.  
  
> [!NOTE]  
>  Valeurs CLOB peuvent être utilisées avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005_md.md)] (ou version ultérieure) des types de données de grande valeur. Plus précisément, les types CLOB peuvent être utilisés avec la **varchar (max)** et **nvarchar (max)** des types de données, types d’objets BLOB peuvent être utilisés avec **varbinary (max)** et **image**  des types de données et les types NCLOB peuvent être utilisé avec **ntext** et **nvarchar (max)**.  
  
## <a name="large-value-data-types"></a>Types de données de grande valeur  
 Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], l’utilisation de données de grande valeur types nécessitait un traitement spécial. Les types de données à valeur élevée sont ceux qui dépassent la taille de ligne maximale de 8 Ko. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] introduit un spécificateur max pour **varchar**, **nvarchar**, et **varbinary** des types de données pour permettre le stockage de valeurs aussi grand que 2 ^ 31 octets. Colonnes de la table et [!INCLUDE[tsql](../../includes/tsql_md.md)] variables peuvent spécifier **varchar (max)**, **nvarchar (max)**, ou **varbinary (max)** des types de données.  
  
 Les principaux scénarios de travail sur des types de données de grande valeur impliquent l'extraction d'une base de données ou l'ajout à une base de données. Les sections suivantes décrivent les différentes approches de réalisation de ces tâches.  
  
### <a name="retrieving-large-value-types-from-a-database"></a>Extraction de types de données de grande valeur d'une base de données  
 Lorsque vous récupérez un type de données de grande valeur non binaire, tel que le **varchar (max)** type de données, à partir d’une base de données, une approche consiste à lire ces données comme un flux de caractères. Dans l’exemple suivant, la [executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverstatement.md) méthode de la [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) classe est utilisée pour récupérer des données à partir de la base de données et retourner dans un jeu de résultats. Le [getCharacterStream](../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) classe est utilisée pour lire les données de grande valeur du jeu de résultats.  
  
```  
ResultSet rs = stmt.executeQuery("SELECT TOP 1 * FROM Test1");  
rs.next();  
Reader reader = rs.getCharacterStream(2);  
```  
  
> [!NOTE]  
>  Cette même approche peut également être utilisée pour le **texte**, **ntext**, et **nvarchar (max)** des types de données.  
  
 Lorsque vous récupérez un type de données de grande valeur binaire, tel que le **varbinary (max)** type de données, à partir d’une base de données, il existe plusieurs approches s’offrent à vous. La plus efficace consiste à lire les données en tant que flux de données binaire, comme suit :  
  
```  
ResultSet rs = stmt.executeQuery("SELECT photo FROM mypics");  
rs.next();  
InputStream is = rs.getBinaryStream(2);  
```  
  
 Vous pouvez également utiliser le [getBytes](../../connect/jdbc/reference/getbytes-method-sqlserverresultset.md) méthode pour lire les données en tant que tableau d’octets, comme suit :  
  
```  
ResultSet rs = stmt.executeQuery("SELECT photo FROM mypics");  
rs.next();  
byte [] b = rs.getBytes(2);  
```  
  
> [!NOTE]  
>  Vous pouvez également lire les donnés en tant que BLOB. Cependant, cette méthode est moins efficace que les deux précédentes.  
  
### <a name="adding-large-value-types-to-a-database"></a>Ajout de types de données de grande valeur à une base de données  
 Le téléchargement de données de grande valeur avec le pilote JDBC fonctionne correctement dans les cas de correspondance avec la taille de la mémoire ; dans les cas de dépassement de la taille de la mémoire, la transmission de flux de données en continu constitue l'option principale. Cependant, la méthode la plus efficace consiste à télécharger des données de grande valeur via les interfaces de flux.  
  
 L'utilisation d'une chaîne ou d'octets est également une option, comme suit :  
  
```  
PreparedStatement pstmt = con.prepareStatement("INSERT INTO test1 (c1_id, c2_vcmax) VALUES (?, ?)");  
pstmt.setInt(1, 1);  
pstmt.setString(2, htmlStr);  
pstmt.executeUpdate();  
```  
  
> [!NOTE]  
>  Cette approche peut également être utilisée pour les valeurs qui sont stockés dans **texte**, **ntext**, et **nvarchar (max)** colonnes.  
  
 Si vous avez une bibliothèque d’images sur le serveur et que vous devez télécharger des fichiers image binaires à une **varbinary (max)** colonne, la méthode la plus efficace avec le pilote JDBC est d’utiliser les flux directement, comme dans l’exemple suivant :  
  
```  
PreparedStatement pstmt = con.prepareStatement("INSERT INTO test1 (Col1, Col2) VALUES(?,?)");  
File inputFile = new File("CLOBFile20mb.jpg");  
FileInputStream inStream = new FileInputStream(inputFile);  
int id = 1;  
pstmt.setInt(1,id);  
pstmt.setBinaryStream(2, inStream);  
pstmt.executeUpdate();  
inStream.close();  
```  
  
> [!NOTE]  
>  L'utilisation de la méthode CLOB ou BLOB n'est pas efficace pour le téléchargement de données de grande valeur.  
  
### <a name="modifying-large-value-types-in-a-database"></a>Modification de types de données de grande valeur dans une base de données  
 Dans la plupart des cas, la méthode recommandée pour la mise à jour ou de la modification de grande valeur dans la base de données est à transmettre des paramètres via la [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) et [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classes à l’aide de [!INCLUDE[tsql](../../includes/tsql_md.md)] commandes telles que UPDATE, WRITE et SUBSTRING.  
  
 Si vous devez remplacer l’instance d’un mot dans un fichier texte volumineux, tel qu’un fichier HTML archivé, vous pouvez utiliser un objet Clob, comme suit :  
  
```  
String SQL = "SELECT * FROM test1;";  
Statement stmt = con.createStatement(ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet.CONCUR_UPDATABLE);  
ResultSet rs = stmt.executeQuery(SQL);  
rs.next();  
  
Clob clob = rs.getClob(2);  
long pos = clob.position("dog", 1);  
clob.setString(pos, "cat");  
rs.updateClob(2, clob);  
rs.updateRow();  
```  
  
 De plus, vous pouvez effectuer tout le travail sur le serveur et simplement transmettre les paramètres à une instruction UPDATE préparée.  
  
 Pour plus d'informations sur les types de données de grande valeur, consultez « Utilisation de types de données de grande valeur » dans la documentation en ligne de SQL Server.  
  
## <a name="xml-data-type"></a>Type de données XML  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] fournit un **xml** type de données qui vous permet de stocker des documents XML et des fragments dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données. Le **xml** type de données est un type de données intégré dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]et est quelque peu similaire aux autres types intégrés, tels que **int** et **varchar**. Comme avec d’autres types intégrés, vous pouvez utiliser la **xml** type de données comme type de colonne lorsque vous créez une table, comme un type de variable, un type de paramètre ou un type de retour de fonction ou dans [!INCLUDE[tsql](../../includes/tsql_md.md)] fonctions CAST et CONVERT.  
  
 Dans le pilote JDBC, le **xml** type de données peut être mappé comme chaîne, tableau d’octets, flux, CLOB, BLOB ou objet SQLXML. Chaîne est la valeur par défaut. À partir de la version 2.0 du pilote JDBC, l'API JDBC 4.0 est prise en charge, ce qui permet l'introduction de l'interface SQLXML. L’interface SQLXML définit des méthodes pour interagir et manipuler des données XML. Le **SQLXML** type de données correspond à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **xml** type de données. Pour plus d’informations sur comment lire et écrire des données XML depuis et vers la base de données relationnelle avec le **SQLXML** type de données Java, consultez [prise en charge des données XML](../../connect/jdbc/supporting-xml-data.md).  
  
 L’implémentation de la **xml** type de données dans le pilote JDBC fournit la prise en charge pour les éléments suivants :  
  
-   Accès à XML en tant que chaîne standard Java UTF-16 pour les scénarios de programmation les plus courants  
  
-   Entrée de XML en UTF-8 ou codé sur 8 bits  
  
-   Accès à XML en tant que tableau d'octets avec un BOM de début lors de l'encodage en UTF-16 pour l'échange avec d'autres processeurs XML et fichiers de disque  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] requiert un BOM de début pour le XML codé en UTF-16. L'application doit le fournir quand les valeurs de paramètre XML sont fournies en tant que tableaux d'octets. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Retourne toujours les valeurs XML comme UTF-16 sans BOM des chaînes ou déclaration de codage incorporée. Lorsque des valeurs XML sont récupérées en tant que byte[], BinaryStream ou Blob, une marque d'ordre d'octet (BOM, Byte-Order Mark) UTF-16 est ajoutée au début de la valeur.  
  
 Pour plus d’informations sur la **xml** de type de données, consultez « Type de données xml » dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="user-defined-data-type"></a>Type de données défini par l'utilisateur  
 L’introduction des types définis par l’utilisateur (UDT) dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005_md.md)] étend le système de type SQL en vous permettant de stocker des objets et des structures de données personnalisées dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données. Les UDT peuvent contenir plusieurs types de données et avoir des comportements, ce qui les différencie des types de données d'alias traditionnels qui ne comportent qu'un seul type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Les types UDT sont définis à l'aide d'un des langages pris en charge par le CLR (Common Language Runtime) Microsoft .NET produisant un code vérifiable. Cela inclut Microsoft Visual C# et Visual Basic .NET. Les données sont exposées en tant que champs et propriétés d'une classe ou structure basée sur le .NET Framework ; par ailleurs, les comportements sont définis par les méthodes de la classe ou de la structure.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], un UDT peut être utilisé en tant que définition de colonne d’une table, en tant que variable dans un [!INCLUDE[tsql](../../includes/tsql_md.md)] lot, ou en tant qu’argument d’un [!INCLUDE[tsql](../../includes/tsql_md.md)] fonction ou procédure stockée.  
  
 Pour plus d’informations sur les types de données définis par l’utilisateur, consultez « Utilisation et modification des Instances de Types définis par l’utilisateur » dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation des types de données du pilote JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md)  
  
  
