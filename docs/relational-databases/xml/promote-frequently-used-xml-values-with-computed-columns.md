---
title: Promouvoir les valeurs XML les plus fréquentes à l’aide de colonnes calculées | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- promoting properties [XML in SQL Server]
- property promotion [XML in SQL Server]
ms.assetid: f5111896-c2fd-4209-b500-f2baa45489ad
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b02df9c27dfc406bec621d68936ac85b6b949406
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="promote-frequently-used-xml-values-with-computed-columns"></a>Promouvoir les valeurs XML les plus fréquentes à l'aide de colonnes calculées
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Si les requêtes portent essentiellement sur un petit nombre de valeurs d'élément et d'attribut, vous pouvez promouvoir ces quantités dans les colonnes relationnelles. Cela peut s'avérer utile lorsque les requêtes sont émises sur une petite partie des données XML et que l'ensemble de l'instance XML est récupéré. Il n'est pas nécessaire de créer un index XML sur la colonne XML. En revanche, la colonne promue peut être indexée. Les requêtes doivent être écrites en vue de l'utilisation de la colonne promue afin que l'optimiseur de requête ne redirige plus les requêtes de la colonne XML vers la colonne promue.  
  
 La colonne promue peut être une colonne calculée de la même table ou une colonne distincte, gérée par l'utilisateur, d'une autre table. Cela suffit lorsque des valeurs singleton sont promues à partir de chaque instance XML. Toutefois, en cas de propriétés à valeurs multiples, vous devez créer une table distincte pour la propriété, comme l'explique la section suivante.  
  
## <a name="computed-column-based-on-the-xml-data-type"></a>Colonne calculée basée sur le type de données xml  
 Une colonne calculée peut être créée à l’aide d’une fonction définie par l’utilisateur qui appelle des méthodes du type de données **xml** . Le type de la colonne calculée peut être n'importe quel type SQL, y compris XML. L'exemple suivant illustre ce concept.  
  
### <a name="example-computed-column-based-on-the-xml-data-type-method"></a>Exemple : colonne calculée basée sur la méthode du type de données xml  
 Créez la fonction définie par l'utilisateur pour extraire le numéro ISBN d'un livre :  
  
```  
CREATE FUNCTION udf_get_book_ISBN (@xData xml)  
RETURNS varchar(20)  
BEGIN  
   DECLARE @ISBN   varchar(20)  
   SELECT @ISBN = @xData.value('/book[1]/@ISBN', 'varchar(20)')  
   RETURN @ISBN   
END  
```  
  
 Ajoutez une colonne calculée à la table pour le numéro ISBN :  
  
```  
ALTER TABLE      T  
ADD   ISBN AS dbo.udf_get_book_ISBN(xCol)  
```  
  
 La colonne calculée peut être indexée de manière habituelle.  
  
### <a name="example-queries-on-a-computed-column-based-on-xml-data-type-methods"></a>exemple: requêtes sur une colonne calculée basée sur les méthodes du type de données xml  
 Pour obtenir l'élément <`book`> portant le numéro ISBN 0-7356-1588-2 :  
  
```  
SELECT xCol  
FROM   T  
WHERE  xCol.exist('/book/@ISBN[. = "0-7356-1588-2"]') = 1  
```  
  
 La requête portant sur la colonne XML peut être réécrite de façon à utiliser directement la colonne calculée comme suit :  
  
```  
SELECT xCol  
FROM   T  
WHERE  ISBN = '0-7356-1588-2'  
```  
  
 Vous pouvez créer une fonction définie par l’utilisateur pour renvoyer le type de données **xml** et une colonne calculée à l’aide de la fonction définie par l’utilisateur. Toutefois, vous ne pouvez pas créer d'index XML sur la colonne XML calculée.  
  
## <a name="creating-property-tables"></a>Création de tables de propriétés  
 Vous pouvez promouvoir certaines des propriétés à valeurs multiples de vos données XML dans une ou plusieurs tables, placer des index sur ces tables et modifier la cible de vos requêtes de façon à les utiliser. C'est généralement l'attitude à adopter lorsqu'un petit nombre de propriétés couvre la plupart de la charge des requêtes. Vous pouvez effectuer les opérations suivantes :  
  
-   Créez une ou plusieurs tables pour enregistrer les propriétés à valeurs multiples. Vous trouverez plus judicieux de stocker une propriété par table et de dupliquer la clé primaire de la table de base dans les tables de propriétés pour conserver la jointure avec la table de base.  
  
-   Si vous voulez conserver l'ordre relatif des propriétés, vous devez introduire une colonne distincte pour l'ordre relatif.  
  
-   Créez des déclencheurs sur la colonne XML pour assurer la maintenance des tables de propriétés. Dans les déclencheurs, procédez ainsi :  
  
    -   Utilisez des méthodes du type de données **xml** , comme **nodes()** et **value()**, pour insérer et supprimer des lignes dans les tables de propriétés.  
  
    -   Créez des fonctions table multidiffusion dans CLR (Common Language Runtime) pour insérer et supprimer des lignes dans les tables de propriétés.  
  
    -   Écrivez des requêtes qui accèdent par SQL aux tables de propriétés et par XML à la colonne XML de la table de base, et prévoyez des jointures entre les tables à l'aide de leur clé primaire.  
  
### <a name="example-create-a-property-table"></a>Exemple : création d'une table de propriétés  
 Supposons, dans cet exemple, que vous voulez promouvoir le prénom des auteurs. Dans la mesure où les livres peuvent avoir un ou plusieurs auteurs, le prénom est une propriété à valeurs multiples. Chaque prénom est stocké dans une ligne distincte d'une table de propriétés. La clé primaire de la table de base est dupliquée dans la table de propriétés pour la jointure en retour avec la table de base.  
  
```  
create table tblPropAuthor (propPK int, propAuthor varchar(max))  
```  
  
### <a name="example-create-a-user-defined-function-to-generate-a-rowset-from-an-xml-instance"></a>exemple : création d'une fonction définie par l'utilisateur pour générer un ensemble de lignes à partir d'une instance XML  
 La fonction table suivante, udf_XML2Table, accepte une valeur de clé primaire et une instance XML. Elle récupère le prénom de tous les auteurs des éléments <`book`> et renvoie un ensemble de lignes pour les paires clé primaire/prénom.  
  
```  
create function udf_XML2Table (@pk int, @xCol xml)  
returns @ret_Table table (propPK int, propAuthor varchar(max))  
with schemabinding  
as  
begin  
      insert into @ret_Table   
      select @pk, nref.value('.', 'varchar(max)')  
      from   @xCol.nodes('/book/author/first-name') R(nref)  
      return  
end  
```  
  
### <a name="example-create-triggers-to-populate-a-property-table"></a>exemple : création de déclencheurs pour remplir une table de propriétés  
 Le déclencheur insert insère des lignes dans la table de propriétés :  
  
```  
create trigger trg_docs_INS on T for insert  
as  
      declare @wantedXML xml  
      declare @FK int  
      select @wantedXML = xCol from inserted  
      select @FK = PK from inserted  
  
   insert into tblPropAuthor  
   select * from dbo.udf_XML2Table(@FK, @wantedXML)  
```  
  
 Le déclencheur delete supprime des lignes dans la table de propriétés en fonction de la valeur de la clé primaire des lignes supprimées :  
  
```  
create trigger trg_docs_DEL on T for delete  
as  
   declare @FK int  
   select @FK = PK from deleted  
   delete tblPropAuthor where propPK = @FK  
```  
  
 Le déclencheur update supprime les lignes existant dans la table de propriétés qui correspondent à l'instance XML mise à jour et insère les nouvelles à la place :  
  
```  
create trigger trg_docs_UPD  
on T  
for update  
as  
if update(xCol) or update(pk)  
begin  
      declare @FK int  
      declare @wantedXML xml  
      select @FK = PK from deleted  
      delete tblPropAuthor where propPK = @FK  
  
   select @wantedXML = xCol from inserted  
   select @FK = pk from inserted  
  
   insert into tblPropAuthor   
      select * from dbo.udf_XML2Table(@FK, @wantedXML)  
end  
```  
  
### <a name="example-find-xml-instances-whose-authors-have-the-same-first-name"></a>Exemple : recherche des instances XML dont les auteurs portent le même prénom  
 La requête peut être formée sur la colonne XML. Une autre possibilité consiste à rechercher le prénom « David » dans la table de propriétés et à faire une jointure en retour avec la table de base pour renvoyer l'instance XML. Exemple :  
  
```  
SELECT xCol   
FROM     T JOIN tblPropAuthor ON T.pk = tblPropAuthor.propPK  
WHERE    tblPropAuthor.propAuthor = 'David'  
```  
  
### <a name="example-solution-using-the-clr-streaming-table-valued-function"></a>Exemple : solution utilisant la fonction table multidiffusion CLR  
 La solution se compose des étapes suivantes :  
  
1.  Définissez une classe CLR, SqlReaderBase, qui met en œuvre ISqlReader et génère une sortie table multidiffusion par application d'une expression de chemin sur une instance XML.  
  
2.  Créez un assembly et une fonction Transact-SQL définie par l'utilisateur pour démarrer la classe CLR.  
  
3.  Définissez les déclencheurs insert, update et delete à l'aide de la fonction définie par l'utilisateur pour assurer la maintenance de la table de propriétés.  
  
 Pour cela, vous devez commencer par créer la fonction multidiffusion dans CLR. Le type de données **xml** est exposé en tant que classe managée SqlXml dans ADO.NET et prend en charge la méthode **CreateReader()** qui retourne un flux XmlReader.  
  
> [!NOTE]  
>  L'exemple de code de cette section utilise XPathDocument et XPathNavigator, ce qui vous oblige à charger tous les documents XML en mémoire. Si vous utilisez un code similaire dans votre application pour traiter plusieurs documents XML volumineux, ce code n'est pas évolutif. Au lieu de cela, laissez les allocations de mémoire petites et utilisez les interfaces multidiffusion chaque fois que possible. Pour plus d’informations sur les performances, consultez [Architecture d’intégration du CLR](http://msdn.microsoft.com/library/05e4b872-3d21-46de-b4d5-739b5f2a0cf9).  
  
```  
public class c_streaming_xml_tvf {  
   public static ISqlReader streaming_xml_tvf   
(SqlXml xmlDoc, string pathExpression) {  
      return (new TestSqlReaderBase (xmlDoc, pathExpression));  
   }  
}  
  
// Class that implements ISqlReader  
public class TestSqlReaderBase : ISqlReader {  
XPathNodeIterator m_iterator;           
   public SqlChars FirstName;  
// Metadata for current resultset  
private SqlMetaData[] m_rgSqlMetaData;        
  
   public TestSqlReaderBase (SqlXml xmlDoc, string pathExpression) {     
      // Variables for XPath navigation  
      XPathDocument xDoc;  
      XPathNavigator xNav;  
      XPathExpression xPath;  
  
      // Set sql metadata  
      m_rgSqlMetaData = new SqlMetaData[1];  
      m_rgSqlMetaData[0] = new SqlMetaData ("FirstName",    
SqlDbType.NVarChar,50);     
  
      //Set up the Navigator  
      if (!xmlDoc.IsNull)  
          xDoc = new XPathDocument (xmlDoc.CreateReader());  
      else  
          xDoc = new XPathDocument ();  
      xNav = xDoc.CreateNavigator();  
      xPath = xNav.Compile (pathExpression);  
      m_iterator = xNav.Select(xPath);  
   }  
   public bool Read() {  
      bool moreRows = true;  
      if (moreRows = m_iterator.MoveNext())  
         FirstName = new SqlChars (m_iterator.Current.Value);  
      return moreRows;  
   }  
}  
```  
  
 Ensuite, créez un assembly et une fonction [!INCLUDE[tsql](../../includes/tsql-md.md)] définie par l'utilisateur SQL_streaming_xml_tvf (non présentée), correspondant à la fonction CLR streaming_xml_tvf. La fonction définie par l'utilisateur sert à définir la fonction table, CLR_udf_XML2Table, pour la génération de l'ensemble de lignes :  
  
```  
create function CLR_udf_XML2Table (@pk int, @xCol xml)  
returns @ret_Table table (FK int, FirstName varchar(max))  
with schemabinding  
as  
begin  
      insert into @ret_Table   
   select @pk, FirstName   
   FROM   SQL_streaming_xml_tvf (@xCol, '/book/author/first-name')  
      return  
end  
```  
  
 Pour finir, définissez les déclencheurs en suivant l'exemple « Création de déclencheurs pour remplir une table de propriétés », mais remplacez udf_XML2Table par la fonction CLR_udf_XML2Table. Le déclencheur insert est présenté dans l'exemple suivant :  
  
```  
create trigger CLR_trg_docs_INS on T for insert  
as  
   declare @wantedXML xml  
   declare @FK int  
   select @wantedXML = xCol from inserted  
   select @FK = PK from inserted  
  
   insert into tblPropAuthor  
      select *  
   from    dbo.CLR_udf_XML2Table(@FK, @wantedXML)  
```  
  
 Le déclencheur delete est identique à celui utilisé dans la version non CLR. Le déclencheur update, quant à lui, remplace simplement la fonction udf_XML2Table() par CLR_udf_XML2Table().  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser des données XML dans les colonnes calculées](../../relational-databases/xml/use-xml-in-computed-columns.md)  
  
  
