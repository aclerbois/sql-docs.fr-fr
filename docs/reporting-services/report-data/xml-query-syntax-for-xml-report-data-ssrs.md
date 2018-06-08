---
title: Syntaxe de requête XML pour les données de rapport XML (SSRS) | Microsoft Docs
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
- namespaces [Reporting Services]
- data processing extensions [Reporting Services], data sources
- xmldp [Reporting Services]
- XML [Reporting Services], data retrieval
ms.assetid: d203886f-faa1-4a02-88f5-dd4c217181ef
caps.latest.revision: 49
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 24dd0f5497d89f49234dc53e0ad71a7cf2e9ab47
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-query-syntax-for-xml-report-data-ssrs"></a>Syntaxe de requête XML pour les données de rapport XML (SSRS)
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]vous permet de créer des datasets pour des sources de données XML. Après avoir défini une source de données, vous devez créer une requête pour le dataset. Selon le type de données XML sur lesquelles pointent la source de données, vous pouvez créer la requête du dataset en incluant une **requête** XML ou un chemin d’élément. Une **requête** XML commence par une balise **\<Query>** et comprend des espaces de noms et des éléments XML variant selon la source de données. Un chemin d'accès à un élément opère indépendamment des espaces de noms ; il précise les nœuds et les attributs de nœud à utiliser à partir des données XML sous-jacentes avec une syntaxe similaire à la syntaxe XPath. Pour plus d’informations sur les chemins d’éléments, consultez [Syntaxe du chemin vers l’élément pour des données de rapport XML &#40;SSRS&#41;](../../reporting-services/report-data/element-path-syntax-for-xml-report-data-ssrs.md).  
  
 Vous pouvez créer une source de données XML pour les types de données XML suivants :  
  
-   Documents XML désignés par une URL utilisant le protocole HTTP  
  
-   Points de terminaison du service Web retournant des données XML  
  
-   Données XML incorporées  
  
 La manière dont vous spécifiez une **requête** XML ou un chemin d’élément dépend du type des données XML.  
  
 Pour un document XML, la **requête** XML est facultative. Si elle est incluse, elle peut contenir un **ElementPath**XML facultatif. La valeur du **ElementPath** XML utilise la syntaxe de chemin d’élément. Vous incluez la **requête** XML et le **ElementPath** XML pour traiter correctement les espaces de noms quand c’est nécessaire pour les données XML de la source de données.  
  
 Pour un point de terminaison de service web vers lequel pointe une URL de chaîne de connexion, la **requête** XML définit la méthode du service web, l’action SOAP ou les deux. Le fournisseur de données XML crée une demande de service Web qui extrait les données XML à utiliser pour le rapport.  
  
> [!NOTE]  
>  Quand l’espace de noms d’un service web comporte une barre oblique (**/)** , incluez à la fois la méthode du service web et l’action SOAP de sorte que l’extension de traitement des données XML puisse dériver correctement l’espace de noms.  
  
 Pour un document XML incorporé, la **requête** XML définit les données XML incorporées à utiliser, inclut des espaces de noms facultatifs et contient un **ElementPath**XML facultatif.  
  
## <a name="specifying-query-parameters-for-xml-data"></a>Spécification de paramètres de la requête pour des données XML  
 Vous pouvez spécifier des paramètres de la requête pour les documents XML.  
  
-   Pour les requêtes URL, les paramètres de la requête sont inclus en tant que paramètres URL standard.  
  
-   Pour les requêtes du service Web, les paramètres de la requête sont passés à la méthode du service Web. Pour définir un paramètre de requête, utilisez la page **Paramètres** de la boîte de dialogue **Propriétés du dataset** . Pour plus d’informations, consultez [Boîte de dialogue Propriétés du dataset, Paramètres](../../reporting-services/report-data/dataset-properties-dialog-box-parameters.md).  
  
### <a name="example"></a> Exemple  
 Les exemples fournis dans le tableau ci-dessous illustrent la manière d'extraire des données du service Web Report Server, un document XML et des données XML incorporées.  
  
|Source de données XML|Exemple de requête|  
|---------------------|-------------------|  
|Données XML du service web provenant de la méthode <xref:ReportService2010.ReportingService2010.ListChildren%2A> .|`<Query>`<br /><br /> `<Method Name="ListChildren" Namespace="http://schemas.microsoft.com/sqlserver/2005/06/30/reporting/reportingservices" />`<br /><br /> `</Query>`|  
|Données XML du service Web issues de SoapAction.|`<Query xmlns=namespace>`<br /><br /> `<SoapAction>http://schemas/microsoft.com/sqlserver/2005/03/23/reporting/reportingservices/ListChildren</SoapAction>`<br /><br /> `</Query>`|  
|Document XML ou données XML incorporées qui utilisent des espaces de noms.<br /><br /> Élément de requête spécifiant des espaces de noms pour un chemin d'accès à un élément.|`<Query xmlns:es="http://schemas.microsoft.com/StandardSchemas/ExtendedSales">`<br /><br /> `<ElementPath>/Customers/Customer/Orders/Order/es:LineItems/es:LineItem</ElementPath>`<br /><br /> `</Query>`|  
|Document XML incorporé.|`<Query>`<br /><br /> `<XmlData>`<br /><br /> `<Customers>`<br /><br /> `<Customer ID="1">Bobby</Customer>`<br /><br /> `</Customers>`<br /><br /> `</XmlData>`<br /><br /> `<ElementPath>Customer {@}</ElementPath>`<br /><br /> `</Query>`|  
|Document XML qui utilise la valeur par défaut.|*Aucune requête*.<br /><br /> Le chemin d'accès à l'élément provient du document XML lui-même et est indépendant des espaces de noms.|  
  
> [!NOTE]  
>  Le premier exemple de service web répertorie le contenu du serveur de rapports qui utilise la méthode <xref:ReportService2006.ReportingService2006.ListChildren%2A> . Pour exécuter cette requête, vous devez créer une nouvelle source de données et définir la chaîne de connexion sur `http://localhost/reportserver/reportservice2006.asmx`. La méthode <xref:ReportService2006.ReportingService2006.ListChildren%2A> prend deux paramètres : **Item** et **Recursive**. Définissez la valeur par défaut pour **Item** à **/** et pour **Recursive** à **1**.  
  
## <a name="specifying-namespaces"></a>Définition d'espaces de noms  
 Utilisez l’élément **Requête** XML pour spécifier les espaces de noms qui sont utilisés dans les données XML de la source de données. La requête XML suivante utilise l’espace de noms **sales**. Les nœuds **ElementPath** XML pour `sales:LineItems` et `sales:LineItem` utilisent l’espace de noms **sales**.  
  
```  
<Query xmlns:sales=  
"http://schemas.microsoft.com/StandardSchemas/ExtendedSales">  
   <SoapAction>  
      http://schemas.microsoft.com/SalesWebService/ListOrders   
   </SoapAction>  
   <ElementPath>  
      Customers/Customer/Orders/Order/sales:LineItems/sales:LineItem  
   </ElementPath>  
</Query>  
```  
  
 Pour spécifier l’espace de noms du fournisseur de données afin que l’espace de noms par défaut reste vide, utilisez **xmldp**. Cela est illustré par l'exemple suivant.  
  
### <a name="example"></a> Exemple  
 Les exemples suivants utilisent le document XML DPNamespace.xml fourni à titre d'illustration à la suite du tableau ci-après. Ce tableau présente deux exemples de syntaxe ElementPath XML incluant des préfixes d'espaces de noms.  
  
|Élément de requête XML|Champs obtenus dans le dataset|  
|-----------------------|-------------------------------------|  
|\<Query/>|Valeur A : `http://schemas.microsoft.com/...`<br /><br /> Valeur B : `http://schemas.microsoft.com/...`<br /><br /> Valeur C : `http://schemas.microsoft.com/...`|  
|`<xmldp:Query xmlns:xmldp="http://schemas.microsoft.com/sqlserver/2005/02/reporting/XmlDPQuery" xmlns:ns="http://schemas.microsoft.com/...">`<br /><br /> `<xmldp:ElementPath>Root {}/ns:Element2/Node</xmldp:ElementPath>`<br /><br /> `</xmldp:Query>`|Valeur D<br /><br /> Valeur E<br /><br /> Valeur F|  
  
#### <a name="xml-document-dpnamespacexml"></a>Document XML : DPNamespace.xml  
 Vous pouvez copier ce document XML et l'enregistrer dans une URL disponible pour être utilisée en tant que source de données par le Concepteur de rapports : par exemple http://localhost/DPNamespace.xml.  
  
```  
<Root xmlns:ns="http://schemas.microsoft.com/...">  
   <ns:Element1>  
      <Node>Value A</Node>  
      <Node>Value B</Node>  
      <Node>Value C</Node>  
   </ns:Element1>  
   <ns:Element2>  
      <Node>Value D</Node>  
      <Node>Value E</Node>  
      <Node>Value F</Node>  
   </ns:Element2>  
</Root>  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Type de connexion XML &#40;SSRS&#41;](../../reporting-services/report-data/xml-connection-type-ssrs.md)   
 [Didacticiels sur Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md)  
  
  
