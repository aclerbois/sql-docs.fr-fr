---
title: À l’aide des Types de données XML | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|features
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- IRowsetChange interface
- IRowsetUpdate interface
- data access [SQL Server Native Client], xml data type
- SQL Server Native Client OLE DB schema rowsets
- PROVIDER_TYPES rowset
- IColumnsInfo interface
- IRowsetFind interface
- IColumnsRowset interface
- PROCEDURE_PARAMETERS rowset
- SQLNCLI, XML
- xml data type [SQL Server], SQL Server Native Client
- SQL Server Native Client, XML
- IRowset interface
- ISequentialStream interface
- ISSCommandWithParameters interface
- SS_XMLSCHEMA rowset
- SQL Server Native Client OLE DB interfaces
- XML [SQL Server], SQL Server Native Client
- COLUMNS rowset
ms.assetid: a7af5b72-c5c2-418d-a636-ae4ac6270ee5
caps.latest.revision: 44
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e3d2cf487958f09af1825456f605a9f73a44e27e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-xml-data-types"></a>Utilisation de types de données XML
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]introduit un **xml** type de données qui vous permet de stocker des documents XML et des fragments dans un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] base de données. Le **xml** type de données est un type de données intégré dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]et est quelque peu similaire aux autres types intégrés, tels que **int** et **varchar**. Comme avec d’autres types intégrés, vous pouvez utiliser la **xml** de type de données comme type de colonne lors de la création d’une table ; comme un type de variable, un type de paramètre ou un type de retour de fonction ou dans les fonctions CAST et CONVERT.  
  
## <a name="programming-considerations"></a>Éléments de programmation à prendre en considération  
 XML peut être autodescriptif dans ce sens où il peut inclure un en-tête XML (facultatif) qui spécifie l'encodage du document. Par exemple :  
  
 `<?xml version="1.0" encoding="windows-1252"?><doc/>`  
  
 Le standard XML décrit la manière dont un processeur XML est capable de détecter l'encodage utilisé pour un document en examinant les premiers octets du document. Parfois, il est possible que l'encodage spécifié par l'application soit en conflit avec celui spécifié par le document. Dans le cadre des documents passés en tant que paramètres liés, XML est traité en tant que données binaires par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ; aucune conversion n'est donc réalisée et l'analyseur XML peut sans problème exploiter l'encodage spécifié dans le document. Toutefois, pour les données XML liées en tant que données de type WSTR, l'application doit alors garantir que le document est encodé au format Unicode. Ceci peut impliquer le chargement du document dans un DOM, le passage de l'encodage en Unicode et la sérialisation du document. Si cela n'est pas fait, des conversions de données peuvent se produire et générer simultanément des données XML non valides ou endommagées.  
  
 Il existe également un risque potentiel de conflit lorsque vous spécifiez du XML dans des littéraux. Par exemple, les données suivantes ne sont pas valides :  
  
 `INSERT INTO xmltable(xmlcol) VALUES('<?xml version="1.0" encoding="UTF-16"?><doc/>')`  
  
 `INSERT INTO xmltable(xmlcol) VALUES(N'<?xml version="1.0" encoding="UTF-8"?><doc/>')`  
  
## <a name="sql-server-native-client-ole-db-provider"></a>Fournisseur OLE DB SQL Server Native Client  
 DBTYPE_XML est un nouveau type de données propre à XML dans le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. De plus, vous pouvez accéder aux données XML par le biais des types OLE DB existants DBTYPE_BYTES, DBTYPE_WSTR, DBTYPE_BSTR, DBTYPE_XML, DBTYPE_STR, DBTYPE_VARIANT et DBTYPE_IUNKNOWN. Les données stockées dans les colonnes de type XML peuvent être extraites à partir de la colonne d'un ensemble de lignes de fournisseur OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client dans les formats suivants :  
  
-   Une chaîne de texte  
  
-   Un **ISequentialStream**  
  
> [!NOTE]  
>  Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif n’inclut pas le lecteur SAX, mais la **ISequentialStream** peuvent être facilement passés aux objets SAX et DOM dans MSXML.  
  
 **ISequentialStream** doit être utilisée pour la récupération de documents XML volumineux. Les mêmes techniques utilisées pour les types de valeur élevée s'appliquent également à XML. Pour plus d’informations, consultez [à l’aide des Types de valeur volumineux](../../../relational-databases/native-client/features/using-large-value-types.md).  
  
 Les données stockées dans des colonnes de type XML dans un ensemble de lignes peuvent également être extraites, insérées ou mises à jour par une application via des interfaces classiques tels que **IRow::GetColumns**, **IRowChange::SetColumns**, et **ICommand::Execute**. De même pour le cas de la récupération, une application peut passer une chaîne de texte ou un **ISequentialStream** à la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif.  
  
> [!NOTE]  
>  Pour envoyer des données XML sous forme de chaîne via la **ISequentialStream** interface, vous devez vous procurer **ISequentialStream** en spécifiant DBTYPE_IUNKNOWN et définir son *pObject* l’argument de valeur null dans la liaison.  
  
 Lorsque des données XML extraites apparaissent tronquées en raison d'une mémoire tampon du consommateur trop petite, la longueur retournée peut être 0xffffffff, ce qui signifie qu'elle est n'est pas connue. Ceci est cohérent avec son implémentation en tant que type de données transmis au client sans envoyer des informations de longueur avant les données réelles. Dans certains cas la longueur réelle peut être retournée lorsque le fournisseur a mis en mémoire tampon la valeur entière, tels que **IRowset::GetData** et où la conversion de données est effectuée.  
  
 Les données XML envoyées à SQL Server sont traitées en tant que données binaires par le serveur. Ceci empêche tout risque de conversion et permet à l'analyseur XML de détecter automatiquement l'encodage XML. Un plus large éventail de documents XML (par exemple, les documents encodés au format UTF-8) peuvent être acceptés en tant qu'entrées dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Si du contenu XML d'entrée est lié en tant que données du type DBTYPE_WSTR, l'application doit garantir qu'il est déjà encodé en Unicode pour éviter tout risque d'altération suite à des conversions de données indésirables.  
  
### <a name="data-bindings-and-coercions"></a>Liaisons de données et forçages de type  
 Le tableau suivant décrit la liaison et le forçage de type se produit lorsque les types de données répertoriées avec le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **xml** type de données.  
  
|Type de données|Vers le serveur<br /><br /> **XML**|Vers le serveur<br /><br /> **Non-XML**|Depuis le serveur<br /><br /> **XML**|Depuis le serveur<br /><br /> **Non-XML**|  
|---------------|---------------------------|--------------------------------|-----------------------------|----------------------------------|  
|DBTYPE_XML|Passer<sup>6,7</sup>|Erreur<sup>1</sup>|OK<sup>11, 6</sup>|Erreur<sup>8</sup>|  
|DBTYPE_BYTES|Passer<sup>6,7</sup>|N/A<sup>2</sup>|OK <sup>11, 6</sup>|N/A <sup>2</sup>|  
|DBTYPE_WSTR|Passer<sup>6,10</sup>|N/A <sup>2</sup>|OK<sup>4, 6, 12</sup>|N/A <sup>2</sup>|  
|DBTYPE_BSTR|Passer<sup>6,10</sup>|N/A <sup>2</sup>|OK <sup>3</sup>|N/A <sup>2</sup>|  
|DBTYPE_STR|OK<sup>6, 9, 10</sup>|N/A <sup>2</sup>|OK<sup>5, 6, 12</sup>|N/A <sup>2</sup>|  
|DBTYPE_IUNKNOWN|Flux d’octets via **ISequentialStream**<sup>7</sup>|N/A <sup>2</sup>|Flux d’octets via **ISequentialStream**<sup>11</sup>|N/A <sup>2</sup>|  
|DBTYPE_VARIANT (VT_UI1 &#124; VT_ARRAY)|Passer<sup>6,7</sup>|N/A <sup>2</sup>|Néant|N/A <sup>2</sup>|  
|DBTYPE_VARIANT (VT_BSTR)|Passer<sup>6,10</sup>|N/A <sup>2</sup>|OK<sup>3</sup>|N/A <sup>2</sup>|  
  
 <sup>1</sup>si un type de serveur autre que DBTYPE_XML est spécifié avec **ICommandWithParameters::SetParameterInfo** et le type d’accesseur est DBTYPE_XML, une erreur se produit lorsque l’instruction est exécutée (DB_E_ERRORSOCCURRED, l’état du paramètre est DBSTATUS_E_BADACCESSOR) ; sinon, les données sont envoyées au serveur, mais le serveur retourne une erreur indiquant qu’il n’existe aucune conversion implicite à partir de XML pour le type de données du paramètre.  
  
 <sup>2</sup>dépasse le cadre de cette rubrique.  
  
 <sup>3</sup>format est UTF-16 sans marque d’ordre bye (octet), aucune spécification d’encodage, aucune marque de fin null.  
  
 <sup>4</sup>format n’est UTF-16, aucune marque BOM, aucune spécification d’encodage, le caractère null de fin.  
  
 <sup>5</sup>format est caractères multioctets encodés dans la page de codes client avec une marque de fin null. Toute conversion à partir de code Unicode sur le serveur risque d'endommager les données. Cette liaison est donc fortement déconseillée.  
  
 <sup>6</sup>BY_REF peut être utilisé.  
  
 <sup>7</sup>données UTF-16 doivent commencer par une nomenclature. Si cela n'est pas le cas, il est possible que l'encodage ne soit pas correctement reconnu par le serveur.  
  
 <sup>8</sup>validation peut se produire à créer l’accesseur ou au moment de l’extraction. L'erreur est DB_E_ERRORSOCCURRED et l'état de la liaison est défini sur DBBINDSTATUS_UNSUPPORTEDCONVERSION.  
  
 <sup>9</sup>données sont converties en Unicode à l’aide de la page de codes du client avant d’être envoyées au serveur. Si l'encodage du document ne correspond pas à la page de codes du client, les données risquent d'être endommagées. Cette liaison est donc fortement déconseillée.  
  
 <sup>10</sup>un BOM est toujours ajoutée aux données envoyées au serveur. Si les données commencent déjà par une marque d'ordre d'octet, deux marques d'ordre d'octet apparaissent alors au démarrage de la mémoire tampon. Le serveur utilise le premier octet à reconnaître l’encodage au format UTF-16 et puis les ignore. La deuxième marque d'ordre d'octet est interprétée comme un espace insécable de largeur nulle.  
  
 <sup>11</sup>format est UTF-16, aucune spécification d’encodage, un BOM est ajouté aux données reçues à partir du serveur. Si une chaîne vide est retournée par le serveur, un BOM est toujours retourné à l’application. Si la longueur de la mémoire tampon est un nombre impair d’octets, les données sont tronquées correctement. Si la valeur entière est retournée en plusieurs segments, ils peuvent être concaténés pour reconstituer la valeur correcte.  
  
 <sup>12</sup>si la longueur de la mémoire tampon est inférieur à deux caractères, pas assez d’espace pour le caractère null de fin : une erreur de dépassement de capacité est signalée.  
  
> [!NOTE]  
>  Aucune donnée n'est retournée pour les valeurs XML NULL.  
  
 Le standard XML exigent que les données XML encodées UTF-16 commencent par une marque d'ordre d'octet (BOM), soit le code de caractère UTF-16 0xFEFF. Lorsque vous travaillez avec des liaisons WSTR et BSTR, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ne requiert pas ou ajouter une nomenclature puisque l’encodage est implicitement par la liaison. Dans le cadre des liaisons BYTES, XML ou IUNKNOWN, l'objectif recherché est la simplification de l'utilisation d'autres processeurs et systèmes de stockage XML. Dans ce cas, une marque d'ordre d'octet doit être présente avec les données XML encodées UTF-16 et l'application n'a pas à se soucier de l'encodage réel puisque la majorité des processeurs XML (y compris SQL Server) détectent l'encodage par inspection des premiers octets de la valeur. Données XML reçues de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client à l’aide de BYTES, XML ou IUNKNOWN des liaisons est toujours encodées en UTF-16 avec une nomenclature et sans déclaration d’encodage incorporée.  
  
 Conversions de données fournies par les services principaux OLE DB (**IDataConvert**) ne sont pas applicables à DBTYPE_XML.  
  
 La validation s'effectue lorsque les données sont envoyées au serveur. La validation côté client et les modifications d'encodage doivent être gérées par votre application et il est fortement recommandé que vous ne traitiez pas les données XML directement mais utilisiez à la place un lecteur DOM ou SAX pour leur traitement.  
  
 Les types DBTYPE_NULL et DBTYPE_EMPTY peuvent être liés pour des paramètres d'entrée mais pas pour des paramètres de résultats ou pour des sorties. S'ils sont liés pour des paramètres d'entrée, l'état doit être défini sur DBSTATUS_S_ISNULL ou DBSTATUS_S_DEFAULT.  
  
 DBTYPE_XML peut être converti en DBTYPE_EMPTY et DBTYPE_NULL, DBTYPE_EMPTY peut être converti en DBTYPE_XML mais DBTYPE_NULL ne peut pas être converti en DBTYPE_XML, ce qui est cohérent avec DBTYPE_WSTR.  
  
 DBTYPE_IUNKNOWN est une liaison prise en charge (voir le tableau ci-dessus) mais il n'existe aucune conversion entre DBTYPE_XML et DBTYPE_IUNKNOWN. DBTYPE_IUNKNOWN ne peut pas être utilisé avec DBTYPE_BYREF.  
  
### <a name="ole-db-rowset-additions-and-changes"></a>Ajout et modifications dans les ensembles de lignes OLE DB  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ajoute de nouvelles valeurs ou les modifications apportées à un grand nombre des principaux ensembles de lignes de schéma OLE DB.  
  
#### <a name="the-columns-and-procedureparameters-schema-rowsets"></a>Ensembles de lignes de schéma COLUMNS et PROCEDURE_PARAMETERS  
 Les ajouts réalisés dans les ensembles de lignes de schéma COLUMNS et PROCEDURE_PARAMETERS incluent les colonnes suivantes.  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|SS_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Nom d'un catalogue dans lequel une collection de schémas XML est définie. Possède la valeur NULL pour une colonne non XML ou une colonne XML non typée.|  
|SS_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Nom d'un schéma dans lequel une collection de schémas XML est définie. Possède la valeur NULL pour une colonne non XML ou une colonne XML non typée.|  
|SS_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Nom de la collection de schémas XML. Possède la valeur NULL pour une colonne non XML ou une colonne XML non typée.|  
  
#### <a name="the-providertypes-schema-rowset"></a>Ensemble de lignes de schéma PROVIDER_TYPES  
 Dans l’ensemble de lignes de schéma PROVIDER_TYPES, la valeur COLUMN_SIZE est 0 pour le **xml** type de données, et la valeur DATA_TYPE est DBTYPE_XML.  
  
#### <a name="the-ssxmlschema-schema-rowset"></a>Ensemble de lignes de schéma SS_XMLSCHEMA  
 Un nouvel ensemble de lignes de schéma SS_XMLSCHEMA permettant d'extraire des informations de schéma XML est proposé pour les clients. L'ensemble de lignes SS_XMLSCHEMA contient les colonnes suivantes.  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Catalogue auquel une collection XML appartient.|  
|SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Schéma auquel une collection XML appartient.|  
|SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Le nom de la collection de schémas XML pour les colonnes XML typées, NULL sinon.|  
|TARGETNAMESPACEURI|DBTYPE_WSTR|Espace de noms cible d'un schéma XML.|  
|SCHEMACONTENT|DBTYPE_WSTR|Contenu du schéma XML.|  
  
 L'étendue de chaque schéma XML est définie par nom de catalogue, nom de schéma, nom de collection de schémas et par URI (Uniform Resource Identifier) d'espace de noms cible. Qui plus est, un nouveau GUID nommé DBSCHEMA_XML_COLLECTIONS est également défini. Le nombre de restrictions et les colonnes restreintes pour l'ensemble de lignes de schéma SS_XMLSCHEMA sont définis comme suit.  
  
|GUID|Nombre de restrictions|Colonnes restreintes|  
|----------|----------------------------|------------------------|  
|DBSCHEMA_XML_COLLECTIONS|4|SCHEMACOLLECTION_CATALOGNAME<br /><br /> SCHEMACOLLECTION_SCHEMANAME<br /><br /> SCHEMACOLLECTIONNAME<br /><br /> TARGETNAMESPACEURI|  
  
### <a name="ole-db-property-set-additions-and-changes"></a>Ajouts et modifications effectués dans le jeu de propriétés OLE DB  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ajoute de nouvelles valeurs ou modifie à la plupart de la propriété OLE DB principaux jeux.  
  
#### <a name="the-dbpropsetsqlserverparameter-property-set"></a>Jeu de propriétés DBPROPSET_SQLSERVERPARAMETER  
 Pour prendre en charge la **xml** type de données via OLE DB, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client implémente le nouveau jeu de propriétés DBPROPSET_SQLSERVERPARAMETER, qui contient les valeurs suivantes.  
  
|Nom|Type| Description|  
|----------|----------|-----------------|  
|SSPROP_PARAM_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Nom d'un catalogue (base de données) dans lequel une collection de schémas XML est définie. Une partie de l’identificateur de nom en trois parties SQL.|  
|SSPROP_PARAM_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Nom d'un schéma XML dans la collection de schémas. Une des trois parties qui composent l'identificateur de nom SQL.|  
|SSPROP_PARAM_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Nom de la collection de schémas XML dans le catalogue. Une des trois parties qui composent l'identificateur de nom SQL.|  
  
#### <a name="the-dbpropsetsqlservercolumn-property-set"></a>Jeu de propriétés DBPROPSET_SQLSERVERCOLUMN  
 Pour prendre en charge la création de tables dans le **ITableDefinition** interface, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ajoute trois nouvelles colonnes au jeu de propriétés DBPROPSET_SQLSERVERCOLUMN.  
  
|Nom|Type| Description|  
|----------|----------|-----------------|  
|SSPROP_COL_XML_SCHEMACOLLECTION_CATALOGNAME|VT_BSTR|Pour les colonnes XML typées, cette propriété est une chaîne qui spécifie le nom du catalogue où le schéma XML est stocké. Pour d'autres types de colonnes, cette propriété retourne une chaîne vide.|  
|SSPROP_COL_XML_SCHEMACOLLECTION_SCHEMANAME|VT_BSTR|Pour les colonnes XML typées, cette propriété est une chaîne qui spécifie le nom de schéma XML qui définit cette colonne.|  
|SSPROP_COL_XML_SCHEMACOLLECTIONNAME|VT_BSTR|Pour les colonnes XML typées, cette propriété est une chaîne qui spécifie le nom de la collection de schémas XML qui définit la valeur.|  
  
 De même que les valeurs SSPROP_PARAM, toutes ces propriétés sont facultatives et sont vides par défaut. Les valeurs SSPROP_COL_XML_SCHEMACOLLECTION_CATALOGNAME et SSPROP_COL_XML_SCHEMACOLLECTION_SCHEMANAME peuvent être spécifiées uniquement si la valeur SSPROP_COL_XML_SCHEMACOLLECTIONNAME est spécifiée. Lorsque vous passez des données XML au serveur, si ces valeurs sont incluses, leur raison d'être (validité) est vérifiée par rapport à la base de données actuelle et les données d'instance sont vérifiées par rapport au schéma. Dans tous les cas, pour être valides, elles doivent toutes être vides ou renseignées.  
  
### <a name="ole-db-interface-additions-and-changes"></a>Ajout et modifications dans l'interface OLE DB  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ajoute de nouvelles valeurs ou modifications apportées à la plupart des interfaces OLE DB principaux.  
  
#### <a name="the-isscommandwithparameters-interface"></a>Interface ISSCommandWithParameters  
 Pour prendre en charge la **xml** type de données via OLE DB, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client implémente un nombre de modifications, y compris l’ajout de la [ISSCommandWithParameters](../../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md) interface. Cette nouvelle interface hérite de l’interface OLE DB **ICommandWithParameters**. Outre les trois méthodes héritées de **ICommandWithParameters**; **GetParameterInfo**, **MapParameterNames**, et **SetParameterInfo**; **ISSCommandWithParameters** fournit le [GetParameterProperties](../../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-getparameterproperties-ole-db.md) et [SetParameterProperties](../../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md) les méthodes qui permettent de gérer des types de données spécifiques de serveur.  
  
> [!NOTE]  
>  Le **ISSCommandWithParameters** interface exploite également SSPARAMPROPS nouvelle structure.  
  
#### <a name="the-icolumnsrowset-interface"></a>Interface IColumnsRowset  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ajoute les éléments suivants [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-colonnes spécifiques à l’ensemble de lignes retourné par la **IColumnRowset::GetColumnsRowset** (méthode). Ces colonnes contiennent le nom en trois parties d'une collection de schémas XML. Dans le cas des colonnes non XML ou des colonnes XML non typées, ces trois colonnes possèdent toutes la valeur NULL par défaut.  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Catalogue auquel une collection de schémas XML appartient.<br /><br /> Possède la valeur NULL sinon.|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Schéma auquel une collection de schémas XML appartient. Possède la valeur NULL sinon.|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Nom d'une collection de schémas XML pour les colonnes XML typées, valeur NULL dans le cas contraire.|  
  
#### <a name="the-irowset-interface"></a>Interface IRowset  
 Une instance XML dans une colonne XML est extraite via la **IRowset::GetData** (méthode). En fonction de la liaison spécifiée par le client, une instance XML peut être extraite en tant que type DBTYPE_BSTR, DBTYPE_WSTR, DBTYPE_VARIANT, DBTYPE_XML, DBTYPE_STR ou DBTYPE_BYTES, ou bien en tant qu'interface via DBTYPE_IUNKNOWN. Si le consommateur spécifie DBTYPE_BSTR, DBTYPE_WSTR ou DBTYPE_VARIANT, le fournisseur convertit l'instance XML d'après le type demandé par l'utilisateur et le stocke à l'emplacement précisé dans la liaison correspondante.  
  
 Si le consommateur spécifie DBTYPE_IUNKNOWN et définit les *pObject* argument NULL, ou ensembles le *pObject* argument IID_ISequentialStream, le fournisseur retourne un **ISequentialStream** d’interface au consommateur afin que le consommateur peut diffuser en continu les données XML de la colonne. **ISequentialStream** puis retourne les données XML comme un flux de caractères Unicode.  
  
 Au moment de retourner une valeur XML liée à DBTYPE_IUNKNOWN, le fournisseur signale une valeur de taille `sizeof (IUnknown *)`. Notez que ceci est cohérent avec l'approche adoptée lorsqu'une colonne est liée en tant que DBTYPE_IUnknown ou DBTYPE_IDISPATCH et par DBTYPE_IUNKNOWN/ISequentialStream lorsque la taille de colonne exacte est impossible à déterminer.  
  
#### <a name="the-irowsetchange-interface"></a>Interface IRowsetChange  
 Il existe deux moyens pour un consommateur de mettre à jour une instance XML dans une colonne. La première est via l’objet de stockage **ISequentialStream** créé par le fournisseur. Le consommateur peut appeler le **ISequentialStream::Write** méthode pour mettre à jour directement l’instance XML retourné par le fournisseur.  
  
 La deuxième approche consiste à utiliser **IRowsetChange::SetData** ou **IRowsetChange::InsertRow** méthodes. Dans ce cas précis, une instance XML issue de la mémoire tampon du consommateur peut être spécifiée dans une liaison de type DBTYPE_BSTR, DBTYPE_WSTR, DBTYPE_VARIANT, DBTYPE_XML ou DBTYPE_IUNKNOWN.  
  
 Dans le cas du type DBTYPE_BSTR, DBTYPE_WSTR ou DBTYPE_VARIANT, le fournisseur stocke l'instance XML résidant dans la mémoire tampon du consommateur à l'intérieur de la colonne appropriée.  
  
 En cas de DBTYPE_IUNKNOWN/ISequentialStream, si le consommateur ne spécifie pas de n’importe quel objet de stockage, le consommateur doit créer un **ISequentialStream** de l’objet à l’avance, lier le document XML avec l’objet, puis passez l’objet au fournisseur via le **IRowsetChange::SetData** (méthode). Le consommateur peut également créer un stockage d’objet, définir l’argument pObject sur IID_ISequentialStream, créer un **ISequentialStream** de l’objet, puis passez la **ISequentialStream** de l’objet à la **IRowsetChange::SetData** (méthode). Dans les deux cas, le fournisseur peut récupérer l’objet XML via le **ISequentialStream** de l’objet et l’insérer dans une colonne appropriée.  
  
#### <a name="the-irowsetupdate-interface"></a>Interface IRowsetUpdate  
 **IRowsetUpdate** interface fournit une fonctionnalité pour les mises à jour retardées. Les données mises à disposition les ensembles de lignes ne sont pas rendues disponibles à d’autres transactions jusqu'à ce que le consommateur appelle la **IRowsetUpdate : Update** (méthode).  
  
#### <a name="the-irowsetfind-interface"></a>Interface IRowsetFind  
 Le **IRowsetFind::FindNextRow** méthode ne fonctionne pas avec le **xml** type de données. Lorsque **IRowsetFind::FindNextRow** est appelée et la *hAccessor* argument spécifie une colonne de type DBTYPE_XML, la valeur DB_E_BADBINDINFO est retournée. Cette situation se produit quel que soit le type de colonne recherché. Pour tout autre type de liaison, le **FindNextRow** échoue avec DB_E_BADCOMPAREOP si la colonne à rechercher est de le **xml** type de données.  
  
## <a name="sql-server-native-client-odbc-driver"></a>Pilote ODBC SQL Server Native Client  
 Dans le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client, un nombre de modifications ont été apportée à des fonctions différentes pour prendre en charge la **xml** type de données.  
  
### <a name="sqlcolattribute"></a>SQLColAttribute  
 Le [SQLColAttribute](../../../relational-databases/native-client-odbc-api/sqlcolattribute.md) fonction a trois nouveaux identificateurs de champ, y compris SQL_CA_SS_XML_SCHEMACOLLECTION_CATALOG_NAME, SQL_CA_SS_XML_SCHEMACOLLECTION_SCHEMA_NAME et SQL_CA_SS _XML_SCHEMACOLLECTION_NAME.  
  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client indique SQL_SS_LENGTH_UNLIMITED pour les colonnes de colonnes SQL_DESC_DISPLAY_SIZE et SQL_DESC_LENGTH.  
  
### <a name="sqlcolumns"></a>SQLColumns  
 Le [SQLColumns](../../../relational-databases/native-client-odbc-api/sqlcolumns.md) fonction a trois nouvelles colonnes, y compris SS_XML_SCHEMACOLLECTION_CATALOG_NAME, SS_XML_SCHEMACOLLECTION_SCHEMA_NAME et SS_XML_SCHEMACOLLECTION_NAME. La colonne TYPE_NAME existante est utilisée pour indiquer le nom du type XML et la valeur DATA_TYPE d'une colonne ou d'un paramètre de type XML est SQL_SS_XML.  
  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client indique SQL_SS_LENGTH_UNLIMITED pour les valeurs COLUMN_SIZE et CHAR_OCTET_LENGTH.  
  
### <a name="sqldescribecol"></a>SQLDescribeCol  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client indique SQL_SS_LENGTH_UNLIMITED lorsque la taille de la colonne ne peut pas être déterminée dans le [SQLDescribeCol](../../../relational-databases/native-client-odbc-api/sqldescribecol.md) (fonction).  
  
### <a name="sqlgettypeinfo"></a>SQLGetTypeInfo  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client indique SQL_SS_LENGTH_UNLIMITED comme valeur COLUMN_SIZE maximale pour le **xml** de type de données dans le [SQLGetTypeInfo](../../../relational-databases/native-client-odbc-api/sqlgettypeinfo.md) (fonction).  
  
### <a name="sqlprocedurecolumns"></a>SQLProcedureColumns  
 Le [SQLProcedureColumns](../../../relational-databases/native-client-odbc-api/sqlprocedurecolumns.md) fonction a les mêmes ajouts de colonne en tant que le **SQLColumns** (fonction).  
  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client indique SQL_SS_LENGTH_UNLIMITED comme valeur COLUMN_SIZE maximale pour le **xml** type de données.  
  
### <a name="supported-conversions"></a>Conversions prises en charge  
 Lorsque vous procédez à des conversions entre des types de données SQL vers C, SQL_C_WCHAR, SQL_C_BINARY et SQL_C_CHAR peuvent tous être convertis en SQL_SS_XML avec les caractéristiques suivantes :  
  
-   SQL_C_WCHAR : format UTF-16, absence de marque d'ordre d'octet (BOM), marque de fin null.  
  
-   SQL_C_BINARY : format UTF-16, absence de marque de fin null. Une marque d'ordre d'octet (BOM) est ajoutée aux données reçues du serveur. Si le serveur retourne une chaîne vide, une marque d'ordre d'octet est quand même retournée à l'application. Si la longueur de la mémoire tampon est un nombre impair d'octets, les données sont tronquées comme il se doit. Si la valeur tout entière est retournée en plusieurs segments, vous pouvez les concaténer pour reconstituer la valeur correcte.  
  
-   SQL_C_CHAR : le format employé est celui de caractères multioctets encodés dans la page de codes du client avec une marque de fin null. Toute conversion à partir de code UTF-16 sur le serveur risque d'endommager les données. Cette liaison est donc fortement déconseillée.  
  
 Lorsque vous procédez à des conversions entre des types de données C vers SQL, SQL_C_WCHAR, SQL_C_BINARY et SQL_C_CHAR peuvent tous être convertis en SQL_SS_XML avec les caractéristiques suivantes :  
  
-   SQL_C_WCHAR : une marque d'ordre d'octet (BOM) est toujours ajoutée aux données envoyées au serveur. Si les données commencent déjà par une marque d'ordre d'octet, deux marques d'ordre d'octet apparaissent alors au démarrage de la mémoire tampon. Le serveur utilise la première marque d'ordre d'octet pour reconnaître l'encodage en tant qu'encodage UTF-16, puis l'ignore. La deuxième marque d'ordre d'octet est interprétée comme un espace insécable de largeur nulle.  
  
-   SQL_C_BINARY : aucune conversion n'a lieu et les données sont transmises au serveur en l'état. Les données UTF-16 doivent commencer par une marque d'ordre d'octet (BOM) sans quoi l'encodage risque d'être mal reconnu par le serveur.  
  
-   SQL_C_CHAR : les données sont converties en UTF-16 sur le client et envoyées au serveur uniquement en tant que SQL_C_WCHAR (avec ajout d'une marque d'ordre d'octet compris). Si le contenu XML n'est pas encodé dans la page de codes du client, les données risquent d'être endommagées.  
  
 Le standard XML exigent que les données XML encodées UTF-16 commencent par une marque d'ordre d'octet (BOM), soit le code de caractère UTF-16 0xFEFF. Lorsque vous travaillez avec une liaison SQL_C_BINARY, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ne requiert pas ou ajouter une nomenclature, puisque l’encodage est implicitement par la liaison. L'objectif recherché est la simplification de l'utilisation d'autres processeurs et systèmes de stockage XML. Dans ce cas, une marque d'ordre d'octet doit être présente avec les données XML encodées UTF-16 et l'application n'a pas à se soucier de l'encodage réel puisque la majorité des processeurs XML (y compris [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]) détectent l'encodage par inspection des premiers octets de la valeur. Données XML reçues de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client à l’aide de SQL_C_BINARY liaisons sont toujours encodées en UTF-16 avec une nomenclature et sans déclaration d’encodage incorporée.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-features.md)   
 [ISSCommandWithParameters & #40 ; OLE DB & #41 ;](../../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
