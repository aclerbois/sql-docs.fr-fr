---
title: Type et colonnes de données XML (SQL Server) | Microsoft Docs
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
ms.assetid: 00db8f21-7d4b-4347-ae43-3a7c314d2fa1
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d76704d665b985804b2383690cd7dbeb22189201
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-data-type-and-columns-sql-server"></a>Type et colonnes de données XML (SQL Server)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cette rubrique présente les avantages et les limites du type de données **XML** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et vous aide à choisir comment stocker des données XML.  
  
## <a name="relational-or-xml-data-model"></a>Modèle de données relationnel ou XML  
 Si vos données sont très structurées et s'accompagnent de schémas connus, le modèle relationnel est sans doute le mieux adapté au stockage des données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit les fonctionnalités et les outils dont vous pourrez avoir besoin. En revanche, s'il s'agit de données semi-structurées ou non structurées, ou si la structure est inconnue, mieux vaut envisager la modélisation de ces données.  
  
 XML s'avère une solution de choix si vous souhaitez un modèle indépendant des plateformes pour assurer la portabilité des données à l'aide d'un balisage structurel et sémantique. De plus, cette solution semble mieux adaptée si certaines des conditions suivantes sont réunies :  
  
-   Vos données sont éparses, vous n'en connaissez pas la structure ou la structure de vos données risque d'évoluer considérablement dans l'avenir.  
  
-   Vos données représentent une hiérarchie d'imbrications, et non des références entre entités, et peuvent être récursives.  
  
-   L'ordre est inhérent dans vos données.  
  
-   Vous souhaitez interroger les données ou en mettre à jour certaines en fonction de leur structure.  
  
 Si aucune de ces conditions n'est remplie, il est préférable d'utiliser le modèle de données relationnel. Par exemple, si vos données sont au format XML, alors que votre application n’utilise la base de données que pour stocker et récupérer les données, une colonne **[n]varchar(max)** suffit. Le stockage des données dans une colonne XML apporte d'autres avantages : le moteur peut vérifier que les données sont bien formées ou valides, et vous pouvez lancer des requêtes et des mises à jour à granularité fine sur les données XML.  
  
## <a name="reasons-for-storing-xml-data-in-sql-server"></a>Raisons justifiant le stockage des données XML dans SQL Server  
 Vous trouverez ici quelques bonnes raisons d'opter pour les fonctions XML natives de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au lieu de gérer vos données XML dans le système de fichiers :  
  
-   Vous voulez partager, interroger et modifier vos données XML d'une manière efficace et transactionnelle. Votre application a besoin d'accéder aux données à un niveau très détaillé. Par exemple, vous souhaitez extraire quelques passages d'un document XML, ou insérer une nouvelle section, sans avoir à remplacer l'ensemble du document.  
  
-   Vous disposez de données relationnelles et de données XML et souhaitez garantir une parfaite interopérabilité entre elles au sein de votre application.  
  
-   Vous cherchez un langage permettant d'interroger et de modifier les données d'applications inter-domaines.  
  
-   Vous souhaitez que le serveur vérifie la bonne formation des données et valide éventuellement vos données par rapport à des schémas XML.  
  
-   Vous cherchez à indexer les données XML pour optimiser le traitement des requêtes et faciliter la montée en charge, et voulez profiter d'un optimiseur de requête de tout premier ordre.  
  
-   Vous voulez pouvoir accéder aux données XML par le biais de SOAP, ADO.NET et OLE DB.  
  
-   Vous voulez profiter des fonctions d'administration du serveur de base de données pour gérer vos données XML, en cas de sauvegarde, de récupération et de réplication par exemple.  
  
 Si aucune de ces conditions n’est remplie, vous avez intérêt à stocker vos données dans un type d’objet volumineux non-XML, tel que **[n]varchar(max)** ou **varbinary(max)**.  
  
## <a name="xml-storage-options"></a>Options de stockage XML  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous disposez des options de stockage XML suivantes :  
  
-   Stockage en mode natif dans le type de données **xml**   
  
     Les données sont stockées dans une représentation interne qui conserve le contenu XML des données. Cette représentation interne inclut des informations à propos de la hiérarchie de relations contenant-contenu, l'ordre des documents et les valeurs d'éléments et d'attributs. Plus précisément, le contenu InfoSet des données XML est préservé. Pour plus d’informations sur InfoSet, consultez [http://www.w3.org/TR/xml-infoset](http://go.microsoft.com/fwlink/?LinkId=48843). Le contenu InfoSet n'est pas une copie conforme du texte XML puisque les éléments suivants ne sont pas conservés : espaces non significatifs, ordre des attributs, préfixes d'espace de noms et déclaration XML.  
  
     Pour un type de données **xml** typé, un type de données **xml** lié à des schémas XML, le jeu d’informations de validation post-schéma (PSVI) ajoute des informations de type dans le jeu d’informations et il est encodé dans la représentation interne. L'analyse s'en trouve considérablement accélérée. Pour plus d’informations, consultez les spécifications XML Schema de W3C sur [http://www.w3.org/TR/xmlschema-1](http://go.microsoft.com/fwlink/?LinkId=48881) et [http://www.w3.org/TR/xmlschema-2](http://go.microsoft.com/fwlink/?LinkId=4871).  
  
-   Mappage entre stockage XML et relationnel  
  
     À l'aide d'un schéma annoté (AXSD), le code XML est décomposé en colonnes dans une ou plusieurs tables de façon à préserver la fidélité des données au niveau relationnel. La structure hiérarchique est ainsi conservée bien que l'ordre des éléments soit ignoré. Le schéma ne peut pas être récursif.  
  
-   Stockage d’objets volumineux, **[n]varchar(max)** et **varbinary(max)**  
  
     Une copie conforme des données est stockée. Cela s'avère nécessaire pour des applications spécifiques telles que des documents juridiques. Généralement, les applications ne réclament pas une copie conforme et se satisfont du contenu XML (fidélité de l'InfoSet).  
  
 Dans la plupart des cas, vous aurez probablement à combiner ces deux approches. Vous pouvez, par exemple, stocker vos données XML dans une colonne de type de données **xml** et en promouvoir les propriétés dans des colonnes relationnelles. Vous pouvez également mapper les technologies pour stocker uniquement les parties récursives dans des colonnes de type de données **xml** et les parties non récursives dans des colonnes non-XML.  
  
### <a name="choice-of-xml-technology"></a>Choix de la technologie XML  
 Le choix de la technologie XML, mode XML natif ou vue XML, dépend généralement des facteurs suivants :  
  
-   Option de stockage  
  
     Vos données XML sont peut-être plus adaptées à un stockage d'objet volumineux (manuel de produit, par exemple) ou à un stockage en colonnes relationnelles (article converti en XML, par exemple). Chaque option de stockage préserve la fidélité du document à sa manière.  
  
-   Interrogation des données par requête  
  
     Vous trouverez peut-être qu'une option de stockage convient mieux qu'une autre en fonction de la nature de vos requêtes et de la façon dont vous interrogez vos données XML. Les requêtes à granularité fine sur des données XML, évaluation de prédicat sur des nœuds XML par exemple, ne sont prises en charge qu'à des degrés divers dans les deux options de stockage.  
  
-   Indexation des données XML  
  
     Vous cherchez peut-être à indexer les données XML de manière à optimiser les performances des requêtes XML. Les options d'indexation varient avec les options de stockage. Il vous appartient de choisir la solution la mieux adaptée à l'optimisation de votre charge de travail.  
  
-   Modification des données  
  
     Certaines charges de travail impliquent la modification des données XML à un degré de granularité assez fin. Il peut s'agir, notamment, d'ajouter une nouvelle section à un document sans que d'autres charges de travail, le contenu Web par exemple, ne soient concernées. La prise en charge d'un langage de modification des données peut jouer un rôle essentiel pour votre application.  
  
-   Prise en charge des schémas  
  
     Vos données XML peuvent être décrites par un schéma qui peut (ou non) être un document de schéma XML. La prise en charge de code XML associé à un schéma dépend de la technologie XML adoptée.  
  
 Qui dit choix différents, dit aussi performances différentes.  
  
### <a name="native-xml-storage"></a>Stockage XML natif  
 Vous pouvez stocker vos données XML dans une colonne de type de données **xml** sur le serveur. C'est une solution de choix si vous vous trouvez dans les conditions suivantes :  
  
-   Vous cherchez un moyen simple de stocker vos données XML sur le serveur et voulez, en parallèle, préserver l'ordre et la structure du document.  
  
-   Vous disposez ou non d'un schéma pour vos données XML.  
  
-   Vous voulez pouvoir interroger et modifier vos données XML.  
  
-   Vous voulez indexer les données XML pour accélérer le traitement des requêtes.  
  
-   Votre application a besoin d'affichages catalogue système pour gérer vos données XML et vos schémas XML.  
  
 Le stockage XML natif est utile lorsque les documents XML présentent des structures différentes ou suivent des schémas différents ou complexes beaucoup trop difficiles à mapper à des structures relationnelles.  
  
#### <a name="example-modeling-xml-data-using-the-xml-data-type"></a>Exemple : modélisation de données XML à l'aide du type de données xml  
 Prenons l'exemple d'un manuel de produit au format XML. Chaque rubrique fait l'objet d'un chapitre distinct, lui-même composé de plusieurs sections. Une section peut contenir des sous-sections. L’élément \<section> est donc un élément récursif. Les manuels de produit regroupent un gros volume de données diverses : contenu, diagrammes, explications techniques ; les données sont semi-structurées. Les utilisateurs veulent pouvoir lancer une recherche contextuelle sur les rubriques qui les intéressent, par exemple rechercher la section consacrée aux « index cluster » dans le chapitre sur l'« indexation », et s'informer des quantités techniques.  
  
 Une colonne de type de données **xml** constitue un modèle de stockage approprié pour vos documents XML. Vous conservez ainsi le contenu InfoSet de vos données XML. L'indexation de la colonne XML permet d'optimiser les performances des requêtes.  
  
#### <a name="example-retaining-exact-copies-of-xml-data"></a>Exemple : conservation de copies conformes des données XML  
 Supposons, par exemple, que la législation en vigueur vous oblige à conserver des copies textuelles conformes de vos documents XML. Il pourrait s'agir notamment de documents signés, de documents juridiques ou d'ordres boursiers. Vous pouvez stocker vos documents dans une colonne **[n]varchar(max)** .  
  
 Pour ce qui est des requêtes, convertissez les données en données de type **xml** lors de l'exécution et appliquez-leur une requête Xquery. La conversion lors de l'exécution peut s'avérer onéreuse, surtout si le document est volumineux. Si vous exécutez fréquemment des requêtes, vous pouvez stocker les documents de façon redondante dans une colonne de type de données **xml** , puis indexer cette dernière quand vous retournez des copies conformes à partir de la colonne **[n]varchar(max)** .  
  
 La colonne XML peut être une colonne calculée, basée sur la colonne **[n]varchar(max)** . Toutefois, vous ne pouvez pas créer d’index XML sur une colonne XML calculée, ni créer un index XML à partir de colonnes **[n]varchar(max)** ou **varbinary(max)** .  
  
### <a name="xml-view-technology"></a>Vue XML  
 En définissant un mappage entre vos schémas XML et les tables d'une base de données, vous créez une « vue XML » de vos données persistantes. Le chargement en masse XML peut servir à remplir les tables sous-jacentes d'après la vue XML. Vous pouvez aussi interroger la vue XML en utilisant XPath version 1.0 ; la requête est traduite en requêtes SQL portant sur les tables. De même, les mises à jour peuvent se propager à ces tables.  
  
 Les vues XML sont utiles dans les cas suivants :  
  
-   Vous voulez avoir un modèle de programmation orienté XML en utilisant des vues XML des données relationnelles existantes.  
  
-   Vous avez un schéma (XSD, XDR) pour vos données XML qu'un partenaire externe vous a fourni.  
  
-   L'ordre de vos données importe peu, les données des tables de requête ne sont pas récursives, ou la profondeur de récursivité maximale est connue à l'avance.  
  
-   Vous voulez interroger et modifier les données via la vue XML en utilisant XPath version 1.0.  
  
-   Vous voulez charger en masse les données XML, puis les répartir dans les tables sous-jacentes à l'aide de la vue XML.  
  
 Il pourrait s'agir de données relationnelles exposées au format XML pour l'échange des données et les services Web, et de données XML avec un schéma fixe. Pour plus d'informations, consultez [MSDN Online Library](http://go.microsoft.com/fwlink/?linkid=31174)(éventuellement en anglais).  
  
#### <a name="example-modeling-data-using-an-annotated-xml-schema-axsd"></a>Exemple : modélisation des données à l'aide d'un schéma XML annoté (AXSD)  
 Partons du principe que vous disposez de données relationnelles (clients, commandes et articles) et que vous voulez les gérer sous forme XML. Définissez une vue XML en utilisant AXSD sur les données relationnelles. La vue XML vous permet de charger en masse les données XML dans vos tables, puis d'interroger et de mettre à jour les données relationnelles à l'aide de la vue XML. Ce modèle s'avère très utile si vous avez à échanger des données contenant des balises XML avec d'autres applications alors que les applications SQL fonctionnent sans interruption.  
  
### <a name="hybrid-model"></a>Modèle hybride  
 Bien souvent, la modélisation des données fait intervenir à la fois des colonnes relationnelles et des colonnes de type **xml** . Certaines valeurs de vos données XML peuvent être stockées dans des colonnes relationnelles alors que le reste (ou la totalité) des valeurs XML sont stockées dans une colonne XML. Vous obtenez ainsi de meilleures performances puisque vous avez une meilleure maîtrise des index créés sur les colonnes relationnelles et sur les caractéristiques des verrous.  
  
 Les valeurs à stocker dans les colonnes relationnelles dépendent de votre charge de travail. Par exemple, si vous récupérez toutes les valeurs XML en fonction de l’expression de chemin, /Customer/@CustId, en promouvant la valeur de l’attribut **CustId** dans une colonne relationnelle et en l’indexant, vous pouvez accélérer notablement le traitement des requêtes. En revanche, si vos données XML sont largement décomposées de façon non redondante dans des colonnes relationnelles, le réassemblage risque de s'avérer fort coûteux.  
  
 Pour des données XML très structurées, par exemple, le contenu d'une table a été converti en XML de façon à pouvoir mapper toutes les valeurs aux colonnes relationnelles, et éventuellement faire appel aux vues XML.  
  
## <a name="granularity-of-xml-data"></a>Granularité des données XML  
 La granularité des données XML stockées dans une colonne XML est d'une importance capitale pour le verrouillage et de moindre importance pour les mises à jour. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise le même mécanisme de verrouillage, qu’il s’agisse de données XML ou non XML. Par conséquent, le verrouillage au niveau de la ligne verrouille toutes les instances XML de la ligne. En cas de grosse granularité, le verrouillage des grosses instances XML pendant les mises à jour provoque une baisse de la capacité de traitement dans un scénario multi-utilisateur. En cas de décomposition fine, l'encapsulation des objets est perdue et le coût de réassemblage augmente.  
  
 Pour le bien de la conception, il est essentiel de trouver un juste équilibre entre les exigences de la modélisation des données et les critères de verrouillage et de mise à jour. Toutefois, dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la taille des instances XML stockées proprement dites ne revêt pas une telle importante.  
  
 Par exemple, les mises à jour d'une instance XML s'effectuent à l'aide d'une nouvelle prise en charge des mises à jour partielles d'objets blob et d'index au cours desquelles l'instance XML stockée est comparée à sa version mise à jour. En cas de mises à jour partielles d'objets blob, une comparaison différentielle des deux instances XML a lieu et seules les différences sont mises à jour. En cas de mises à jour partielles d'index, seules les lignes devant être changées dans l'index XML sont modifiées.  
  
## <a name="limitations-of-the-xml-data-type"></a>Limites du type de données xml  
 Notez les limitations générales suivantes applicables au type de données **xml** :  
  
-   La représentation stockée d'instances de type de données **xml** ne peut pas dépasser 2 Go.  
  
-   Il ne peut pas être utilisé comme sous-type d’une instance **sql_variant** .  
  
-   Il ne prend pas en charge la conversion en **text** ni en **ntext**. Utilisez **varchar(max)** ou **nvarchar(max)** à la place.  
  
-   Il ne peut pas être comparé ni trié. Autrement dit, un type de données **xml** ne peut pas être utilisé dans une instruction GROUP BY.  
  
-   Il ne peut pas être utilisé en tant que paramètre d'une fonction scalaire intégrée autre que ISNULL, COALESCE et DATALENGTH.  
  
-   Il ne peut pas être utilisé en tant que colonne clé dans un index. En revanche, il peut être inclus en tant que donnée dans un index cluster ou ajouté explicitement à un index non-cluster à l'aide du mot clé INCLUDE lors de la création d'un index non-cluster.  
  
## <a name="see-also"></a> Voir aussi  
 [Exemples d’importation et d’exportation en bloc de documents XML &#40;SQL Server&#41;](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)  
  
  
