---
title: sp_xml_preparedocument (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_xml_preparedocument_TSQL
- sp_xml_preparedocument
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xml_preparedocument
ms.assetid: 95f41cff-c52a-4182-8ac6-bf49369d214c
caps.latest.revision: 38
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 02946ee1a36df965d11c85eabbaba86d7f1a7e14
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spxmlpreparedocument-transact-sql"></a>sp_xml_preparedocument (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Lit le texte XML fourni en entrée, l'analyse à l'aide de l'analyseur MSXML (Msxmlsql.dll), puis fournit le document analysé dans un état exploitable. Ce document analysé est une représentation arborescente des différents nœuds du document XML : éléments, attributs, textes, commentaires, etc.  
  
 **sp_xml_preparedocument** retourne un handle qui peut être utilisé pour accéder à la représentation interne nouvellement créée du document XML. Ce handle est valide pour la durée de la session ou jusqu'à ce que le handle est invalidé par l’exécution de **sp_xml_removedocument**.  
  
> [!NOTE]  
>  Un document analysé est stocké dans le cache interne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'analyseur MSXML utilise un huitième de la mémoire totale disponible pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour éviter l’exécution de la mémoire, exécutez **sp_xml_removedocument** afin de libérer la mémoire.  
  
> [!NOTE]  
>  Pour en amont la compatibilité, **sp_xml_preparedocument** réduit la demande de modification (char(13)) et LF (char(10)) caractères dans les attributs, même si ces caractères sont décomposés en entités.  
  
> [!NOTE]  
>  L’analyseur XML appelé par **sp_xml_preparedocument** peut analyser des DTD internes et les déclarations d’entité. Étant donné que construits à des fins malveillantes des DTD et les entités déclarations peuvent être utilisées pour effectuer une attaque par déni de service, nous recommandons que les utilisateurs pas passer directement des documents XML à partir de sources non approuvées de **sp_xml_preparedocument**.  
>   
>  Pour atténuer les attaques d’extension d’entité récursive, **sp_xml_preparedocument** limite à 10 000 le nombre d’entités qui peuvent être étendues sous une seule entité au niveau supérieur d’un document. Cette limite ne s'applique pas aux entités numériques ou de caractères. Elle permet de stocker des documents contenant de nombreuses références d'entités, mais elle empêche une entité d'être étendue de manière récursive dans une chaîne supérieure à 10 000 extensions.  
  
> [!NOTE]  
>  **sp_xml_preparedocument** limite le nombre d’éléments pouvant être ouverts simultanément à 256.  

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_xml_preparedocument  
hdoc   
OUTPUT  
[ , xmltext ]  
[ , xpath_namespaces ]   
```  
  
## <a name="arguments"></a>Arguments  
 *hdoc*  
 Descripteur du document nouvellement créé. *hdoc* est un entier.  
  
 [ *xmltext* ]  
 Document XML initial. L'analyseur MSXML analyse ce document XML. *XmlText* est un paramètre de texte : **char**, **nchar**, **varchar**, **nvarchar**, **texte**, **ntext** ou **xml**. Lorsque la valeur est NULL (valeur par défaut), une représentation interne d'un document XML vide est créée.  
  
> [!NOTE]  
>  **sp_xml_preparedocument** peut traiter uniquement de texte ou XML non typé. Si une valeur d'instance à utiliser en entrée est déjà du XML typé, commencez par la convertir en une nouvelle instance XML non typée ou en une chaîne, puis passez cette valeur en entrée. Pour plus d’informations, consultez [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
 [ *xpath_namespaces* ]  
 Spécifie les déclarations d'espaces de noms utilisées dans les expressions XPath de ligne et de colonne sous OPENXML. *xpath_namespaces* est un paramètre de texte : **char**, **nchar**, **varchar**, **nvarchar**, **detexte**, **ntext** ou **xml**.  
  
 La valeur par défaut est  **\<racine xmlns:mp = "urn : schemas-microsoft-com-metaprop » >**. *xpath_namespaces* fournit l’URI d’espace de noms des préfixes utilisés dans les expressions XPath sous OPENXML au moyen d’un document XML bien formé. *xpath_namespaces* déclare le préfixe qui doit être utilisé pour faire référence à l’espace de noms **urn : schemas-microsoft-com-metaprop**; fournit les métadonnées sur les éléments XML analysés. Bien que cette technique vous permette de redéfinir le préfixe de l'espace de noms des métapropriétés, cet espace de noms n'est pas perdu. Le préfixe **mp** est toujours valide pour **urn : schemas-microsoft-com-metaprop** même si *xpath_namespaces* ne contient aucun une telle déclaration.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou >0 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-preparing-an-internal-representation-for-a-well-formed-xml-document"></a>A. Préparation d'une représentation interne pour un document XML correctement mis en forme  
 L'exemple suivant retourne un descripteur pour la représentation interne nouvellement créée du document XML fourni en entrée. L'appel à `sp_xml_preparedocument` contient un mappage de préfixes d'espaces de noms par défaut.  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc;  
-- Remove the internal representation.  
exec sp_xml_removedocument @hdoc;  
```  
  
### <a name="b-preparing-an-internal-representation-for-a-well-formed-xml-document-with-a-dtd"></a>B. Préparation d'une représentation interne pour un document XML correctement mis en forme avec un schéma DTD  
 L'exemple suivant retourne un descripteur pour la représentation interne nouvellement créée du document XML fourni en entrée. La procédure stockée valide le document chargé sur le schéma DTD inclus dans le document. L'appel à `sp_xml_preparedocument` contient un mappage de préfixes d'espaces de noms par défaut.  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(2000);  
SET @doc = '  
<?xml version="1.0" encoding="UTF-8" ?>   
<!DOCTYPE root   
[<!ELEMENT root (Customers)*>  
<!ELEMENT Customers EMPTY>  
<!ATTLIST Customers CustomerID CDATA #IMPLIED ContactName CDATA #IMPLIED>]>  
<root>  
<Customers CustomerID="ALFKI" ContactName="Maria Anders"/>  
</root>';  
  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc;  
```  
  
### <a name="c-specifying-a-namespace-uri"></a>C. Spécification d'un URI d'espace de noms  
 L'exemple suivant retourne un descripteur pour la représentation interne nouvellement créée du document XML fourni en entrée. L’appel à `sp_xml_preparedocument` conserve la `mp` préfixe au mappage d’espace de noms de métapropriétés et ajoute le `xyz` préfixe de mappage de l’espace de noms `urn:MyNamespace`.  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc, '<ROOT xmlns:xyz="urn:MyNamespace"/>';  
```  
  
## <a name="see-also"></a>Voir aussi  
 <br>[Code XML stocké Procedures(Transact-SQL)](../../relational-databases/system-stored-procedures/xml-stored-procedures-transact-sql.md)
 <br>[Procedures(Transact-SQL) stockées du système](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
 <br>[OPENXML(Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)
 <br>[Sys.dm_exec_xml_handles (Transact-SQL)](../system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)
 <br>[sp_xml_removedocument (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)
  
  
