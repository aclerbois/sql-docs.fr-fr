---
title: Syntaxe du chemin à l’élément pour des données de rapport XML (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ElementPath syntax
- XML [Reporting Services], data retrieval
ms.assetid: 07bd7a4e-fd7a-4a72-9344-3258f7c286d1
caps.latest.revision: 43
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: e9ef6761a754f9f25dc47cb033ef491be544ee74
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="element-path-syntax-for-xml-report-data-ssrs"></a>Syntaxe du chemin d'accès à l'élément pour des données de rapport XML (SSRS)
  Dans le Concepteur de rapports, vous spécifiez les données à utiliser pour un rapport à partir d'une source de données XML en définissant un chemin d'accès à l'élément qui respecte la casse. Le chemin d'accès à l'élément indique comment parcourir les nœuds hiérarchiques XML et leurs attributs dans la source de données XML. Pour utiliser le chemin de l’élément par défaut, laissez vide la requête du dataset ou le **ElementPath** XML du **Query** XML. Lorsque les données sont extraites de la source de données XML, les nœuds d'élément possédant des valeurs de texte et des attributs de nœud d'élément deviennent des colonnes dans le jeu de résultats. Les valeurs des nœuds et les attributs deviennent les données de ligne lorsque vous exécutez la requête. Les colonnes apparaissent sous la forme de collection de champs de dataset dans le volet des données de rapport. Cette rubrique décrit la syntaxe du chemin d'accès à l'élément.  
  
> [!NOTE]  
>  La syntaxe du chemin d'accès à l'élément est indépendante de l'espace de noms. Pour utiliser des espaces de noms dans un chemin de l’élément, utilisez la syntaxe de requête XML qui inclut un élément **ElementPath** décrit dans [Syntaxe de requête XML pour les données de rapport XML &#40;SSRS&#41;](../../reporting-services/report-data/xml-query-syntax-for-xml-report-data-ssrs.md).  
  
 Le tableau suivant décrit les conventions utilisées pour définir un chemin d'accès à l'élément.  
  
|Convention|Utilisé pour|  
|----------------|--------------|  
|**gras**|Texte devant être tapé exactement comme indiqué.|  
|&#124; (barre verticale)|Sépare les éléments de la syntaxe. Vous ne pouvez choisir qu'un seul de ces éléments.|  
|`[ ]` (crochets)|Éléments de syntaxe facultatifs. Ne tapez pas les crochets.|  
|**{ }** (accolades)|Délimitent les paramètres des éléments de syntaxe.|  
|[**,**...*n*]|Indique que l’élément précédent peut se répéter *n* fois. Les occurrences sont séparées par des virgules.|  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Element path ::=  
    ElementNode[/Element path]  
ElementNode ::=  
    XMLName[(Encoding)][{[FieldList]}]  
XMLName ::=  
    [NamespacePrefix:]XMLLocalName  
Encoding ::=  
        HTMLEncoded | Base64Encoded  
FieldList ::=  
    Field[,FieldList]  
Field ::=  
    Attribute | Value | Element | ElementNode  
Attribute ::=  
        @XMLName[(Type)]  
Value ::=  
        @[(Type)]  
Element ::=  
    XMLName[(Type)]  
Type ::=  
        String | Integer | Boolean | Float | Decimal | Date | XML   
NamespacePrefix ::=  
    Identifier that specifies the namespace.  
XMLLocalName :: =  
    Identifier in the XML tag.   
```  
  
## <a name="remarks"></a>Notes   
 Le tableau suivant récapitule les termes du chemin d'accès à l'élément. Les exemples du tableau font référence au document XML d'exemple Customers.xml inclus dans la section Exemples de cette rubrique.  
  
> [!NOTE]  
>  Les balises XML respectent la casse. Lorsque vous spécifiez ElementNode dans le chemin d'accès à l'élément, vous devez utiliser exactement les mêmes balises XML que celles de la source de données.  
  
|Terme|Définition|  
|----------|----------------|  
|Element path|Définit la séquence de nœuds à parcourir dans le document XML afin de récupérer les données de champ d'un dataset avec une source de données XML.|  
|**ElementNode**|Nœud XML dans le document XML. Les nœuds sont désignés par des balises et existent dans une relation hiérarchique avec d'autres nœuds. Par exemple, \<Customers> est le nœud de l’élément racine. \<Customer> est un sous-élément de \<Customers>.|  
|**XMLName**|Nom du nœud. Par exemple, le nom du nœud Customers est Customers. **XMLName** peut porter comme préfixe un identificateur d’espace de noms qui identifie de façon unique chaque nœud.|  
|**Encoding**|Indique que **Value** pour cet élément est encodé en XML et doit être décodé et inclus en tant que sous-élément de cet élément.|  
|**FieldList**|Définit l'ensemble des éléments et des attributs à utiliser pour récupérer des données.<br /><br /> Si ce terme n'est pas spécifié, tous les attributs et les sous-éléments sont utilisés comme champs. Si la liste de champs vide est spécifiée (**{}**), aucun champ de ce nœud n’est utilisé.<br /><br /> **FieldList** ne peut pas contenir à la fois **Value** et **Element** ou **ElementNode**.|  
|**Field**|Spécifie les données qui sont extraites en tant que champ de dataset.|  
|**Attribute**|Paire nom-valeur dans **ElementNode**. Par exemple, dans le nœud d’élément \<Customer ID="1">, **ID** est un attribut et **@ID(Integer)** retourne « 1 » comme type d’entier dans l’élément **ID** du champ de données correspondant.|  
|**Value**|Valeur de l'élément. **Value** ne peut être utilisé que sur le dernier **ElementNode** dans le chemin de l’élément. Par exemple, étant donné que \<Return> est un nœud terminal, si vous l’incluez à la fin du chemin d’un élément, la valeur de **Return {@}** est **Chair**.|  
|**Element**|Valeur du sous-élément nommé. Par exemple, Customers {}/Customer {}/LastName récupère des valeurs pour l'élément LastName uniquement.|  
|**Type**|Type de données facultatif utilisé pour le champ créé à partir de cet élément.|  
|**NamespacePrefix**|**NamespacePrefix** est défini dans l’élément de requête XML. S’il n’existe aucun élément de requête XML, les espaces de noms dans **ElementPath** XML sont ignorés. S’il existe un élément de requête XML, **ElementPath** XML possède un attribut **IgnoreNamespaces**facultatif. Si IgnoreNamespaces a la valeur **true**, les espaces de noms dans **ElementPath** XML et le document XML sont ignorés. Pour plus d’informations, consultez [Syntaxe de requête XML pour les données de rapport XML &#40;SSRS&#41;](../../reporting-services/report-data/xml-query-syntax-for-xml-report-data-ssrs.md).|  
  
## <a name="example---no-namespaces"></a>Exemples - Aucun espace de noms  
 Les exemples suivants utilisent le document XML Customers.xml. Ce tableau présente des exemples de syntaxe du chemin d'accès à l'élément et les résultats de l'utilisation du chemin d'accès à l'élément dans une requête qui définit un dataset, en fonction du document XML comme source de données.  
  
> [!NOTE]  
>  Lorsque le chemin d'accès à l'élément est vide, la requête utilise le chemin d'accès à l'élément par défaut : le premier chemin d'accès à une collection de nœuds terminaux. Dans le premier exemple, laisser un chemin d'accès à l'élément vide équivaut à spécifier le chemin d'accès à l'élément /Customers/Customer/Orders/Order. L'ensemble des attributs et des valeurs de nœud, ainsi que le chemin d'accès, sont retournés dans le jeu de résultats, et les noms de nœuds et les noms d'attributs apparaissent en tant que champs du dataset.  
  
 **Exemple #1**: *Vide*  
  
|JSON|Qty|ID|FirstName|LastName|Customer.ID|xmlns|  
|-----------|---------|--------|---------------|--------------|-----------------|-----------|  
|Chair|6| 1|Bobby|Moore|11|http://www.adventure-works.com|  
|Table de charge de travail| 1|2|Bobby|Moore|11|http://www.adventure-works.com|  
|Sofa|2|8|Crystal|Hu|20|http://www.adventure-works.com|  
|EndTables|2|15|Wyatt|Diaz|33|http://www.adventure-works.com|  
  
 **Exemple 2**: `Customers {}/Customer`  
  
|FirstName|LastName|ID|  
|---------------|--------------|--------|  
|Bobby|Moore|11|  
|Crystal|Hu|20|  
|Wyatt|Diaz|33|  
  
 **Exemple 3**: `Customers {}/Customer {}/LastName`  
  
|LastName|  
|--------------|  
|Moore|  
|Hu|  
|Diaz|  
  
 **Exemple 4**: `Customers {}/Customer {}/Orders/Order {@,@Qty}`  
  
|JSON|Qty|  
|-----------|---------|  
|Chair|6|  
|Table de charge de travail| 1|  
|Sofa|2|  
|EndTables|2|  
  
 **Exemple 5**: `Customers {}/Customer/Orders/Order{ @ID(Integer)}`  
  
|Order.ID|FirstName|LastName|ID|  
|--------------|---------------|--------------|--------|  
| 1|Bobby|Moore|11|  
|2|Bobby|Moore|11|  
|8|Crystal|Hu|20|  
|15|Wyatt|Diaz|33|  
  
#### <a name="xml-document-customersxml"></a>Document XML : Customers.xml  
 Pour vous entraîner avec les exemples de chemin d’élément présentés dans la section précédente, vous pouvez copier ce code XML et l’enregistrer dans une URL à laquelle le Concepteur de rapports peut accéder, puis utiliser le document XML comme source de données XML : par exemple, `http://localhost/Customers.xml`.  
  
```  
<?xml version="1.0"?>  
<Customers xmlns="http://www.adventure-works.com">  
   <Customer ID="11">  
      <FirstName>Bobby</FirstName>  
      <LastName>Moore</LastName>  
      <Orders>  
         <Order ID="1" Qty="6">Chair</Order>  
         <Order ID="2" Qty="1">Table</Order>  
      </Orders>  
      <Returns>  
         <Return ID="1" Qty="2">Chair</Return>  
      </Returns>  
   </Customer>  
   <Customer ID="20">  
      <FirstName>Crystal</FirstName>  
      <LastName>Hu</LastName>  
      <Orders>  
         <Order ID="8" Qty="2">Sofa</Order>  
      </Orders>  
      <Returns/>  
   </Customer>  
   <Customer ID="33">  
      <FirstName>Wyatt</FirstName>  
      <LastName>Diaz</LastName>  
      <Orders>  
         <Order ID="15" Qty="2">EndTables</Order>  
      </Orders>  
      <Returns/>  
   </Customer>  
</Customers>  
```  
  
 Vous pouvez également créer une source de données XML ne possédant aucune chaîne de connexion et incorporer Customers.XML dans la requête à l'aide de la procédure suivante :  
  
###### <a name="to-embed-customersxml-in-a-query"></a>Pour incorporer Customers.XML dans une requête  
  
1.  Créez une source de données XML avec une chaîne de connexion vide.  
  
2.  Créez un dataset pour la source de données XML.  
  
3.  Dans la boîte de dialogue **Propriétés du dataset** , cliquez sur **Concepteur de requêtes**. La boîte de dialogue du concepteur de requêtes textuelles s'ouvre.  
  
4.  Dans le volet de requête, entrez les deux lignes suivantes :  
  
     `<Query>`  
  
     `<XmlData>`  
  
5.  Copiez Customers.XML et collez le texte dans le volet de requête après `<XmlData>`.  
  
6.  Dans le volet de requête, supprimez la première ligne que vous avez copiée à partir de Customers.XML : `<?xml version="1.0"?>`  
  
7.  À la fin de la requête, ajoutez les deux lignes suivantes :  
  
     `<XmlData>`  
  
     `<Query>`  
  
8.  Cliquez sur **Exécuter la requête** .  
  
     Le jeu de résultats affiche 4 lignes de données avec les colonnes suivantes : `xmlns`, `Customer.ID`, `FirstName`, `LastName`, `ID`, `Qty`, `Order`.  
  
9. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Type de connexion XML &#40;SSRS&#41;](../../reporting-services/report-data/xml-connection-type-ssrs.md)   
 [Didacticiels sur Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md)   
 [Ajouter, modifier ou actualiser des champs dans le volet des données de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md)  
  
  
