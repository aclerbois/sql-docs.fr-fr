---
title: Introduction au chargement en masse XML (SQLXML 4.0) | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- nontransacted XML Bulk Load operations
- XML Bulk Load [SQLXML], about XML Bulk Load
- bulk load [SQLXML], about bulk load
- transacted XML Bulk Load operations
- streaming XML data
ms.assetid: 38bd3cbd-65ef-4c23-9ef3-e70ecf6bb88a
caps.latest.revision: 13
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ac7e7c9b48f21c3258bb170c1e8f8dea4522eca1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="introduction-to-xml-bulk-load-sqlxml-40"></a>Présentation du chargement en masse XML (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Chargement en masse XML est un objet COM autonome qui vous permet de charger des données XML semi-structurées dans Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tables.  
  
 Vous pouvez insérer les données XML dans une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l'aide d'une instruction INSERT et de la fonction OPENXML ; toutefois, l'utilitaire de chargement en masse fournit de meilleures performances lorsque vous avez besoin d'insérer des quantités importantes de données XML.  
  
 La méthode d’exécution du modèle d’objet de chargement en masse XML accepte deux paramètres :  
  
-   Un schéma XSD (XML Schema Definition)) ou XDR (XML-Data Reduced) annoté. L'utilitaire de chargement en masse XML interprète ce schéma de mappage et les annotations spécifiées dans le schéma pour identifier les tables SQL Server dans lesquelles les données XML doivent être insérées.  
  
-   Un document ou un fragment de document XML (un fragment de document est un document sans élément de niveau supérieur unique). Un nom de fichier ou un flux pouvant être lu par le chargement en masse XML peut être spécifié.  
  
 Le chargement en masse XML interprète le schéma de mappage et identifie le ou les tables dans lesquelles les données XML doivent être insérées.  
  
 Cette rubrique suppose que vous connaissez bien les fonctionnalités [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] suivantes :  
  
-   Schémas XSD et XDR annotés. Pour plus d’informations sur les schémas XSD annotés, consultez [Introduction aux schémas XSD annotés &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md). Pour plus d’informations sur les schémas XDR annotés, consultez [les schémas XDR annotés &#40;déconseillé dans SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
-   Mécanismes d'insertion en bloc [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], tels que l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] BULK INSERT et l'utilitaire bcp. Pour plus d’informations, consultez [BULK INSERT &#40;Transact-SQL&#41; ](../../../t-sql/statements/bulk-insert-transact-sql.md) et [utilitaire bcp](../../../tools/bcp-utility.md).  
  
## <a name="streaming-of-xml-data"></a>Diffusion en continu de données XML  
 Le document XML source pouvant être volumineux, le document n'est pas lu intégralement en mémoire au cours du traitement de chargement en masse. Au lieu de cela, le chargement en masse XML interprète les données XML en tant que flux et lit ce flux. À mesure que l'utilitaire lit les données, il identifie la ou les tables de base de données, génère le ou les enregistrements appropriés à partir de la source de données XML, puis envoie le ou les enregistrements à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à des fins d'insertion.  
  
 Par exemple, le document XML source suivant se compose de  **\<client >** éléments et  **\<ordre >** des éléments enfants :  
  
```  
<Customer ...>  
    <Order.../>  
    <Order .../>  
     ...  
</Customer>  
...  
```  
  
 Comme le chargement en masse XML lit le  **\<client >** élément, il génère un enregistrement pour la table Customertable. Lorsqu’il lit la  **\</Customer >** balise, insère de chargement en masse XML cet enregistrement dans la table de fin [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Dans la même façon, lorsqu’il lit la  **\<ordre >** élément, le chargement en masse XML génère un enregistrement pour le Ordertable, puis insère cet enregistrement dans le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] table lors de la lecture le  **\</Order >** la balise de fin.  
  
## <a name="transacted-and-nontransacted-xml-bulk-load-operations"></a>Opérations de chargement en masse XML transactionnelles et non transactionnelles  
 Le chargement en masse XML peut fonctionner en mode transactionnel ou non transactionnel. Les performances sont généralement optimales si vous effectuez un chargement en masse dans un mode non transactionnel : autrement dit, la propriété de Transaction est définie sur FALSE) et une des conditions suivantes est vraie :  
  
-   Les tables dans lesquelles les données sont chargées en masse sont vides et ne comprennent pas d'index.  
  
-   Les tables ont des données et des index uniques.  
  
 L'approche non transactionnelle ne garantit pas de restauration en cas de problème dans le processus de chargement en masse (bien que des restaurations partielles puissent se produire). Le chargement en masse non transactionnel est approprié lorsque la base de données est vide. Par conséquent, en cas de problème, vous pouvez nettoyer la base de données et redémarrer le chargement en masse XML.  
  
> [!NOTE]  
>  En mode non transactionnel, le chargement en masse XML utilise une transaction interne par défaut et la valide. Lorsque la propriété de Transaction est définie sur TRUE, le chargement en masse XML n’appelle pas de validation sur cette transaction.  
  
 Si la propriété de Transaction est définie sur TRUE, le chargement en masse XML crée des fichiers temporaires, un pour chaque table qui est identifié dans le schéma de mappage. Le chargement en masse XML commence par stocker les enregistrements du document XML source dans ces fichiers temporaires. Ensuite, une instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] BULK INSERT récupère ces enregistrements des fichiers et les stocke dans les tables correspondantes. Vous pouvez spécifier l’emplacement de ces fichiers temporaires à l’aide de la propriété TempFilePath. Vous devez vous assurer que le compte [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilisé avec le chargement en masse XML a accès à ce chemin d'accès. Si la propriété TempFilePath n’est pas spécifiée, le chemin d’accès de fichier par défaut qui est spécifié dans la variable d’environnement TEMP est utilisé pour créer les fichiers temporaires.  
  
 Si la propriété de Transaction est définie sur FALSE (paramètre par défaut), le chargement en masse XML utilise l’interface OLE DB IRowsetFastLoad pour le chargement en masse les données.  
  
 Si la propriété ConnectionString définit la chaîne de connexion et la propriété de Transaction est définie sur TRUE, le chargement en masse XML fonctionne dans son propre contexte de transaction. (Par exemple, le chargement en masse XML démarre sa propre transaction, puis effectue une validation ou une restauration comme il convient.)  
  
 Si la propriété ConnectionCommand définit la connexion avec un objet de connexion existant et la propriété de Transaction est définie sur TRUE, le chargement en masse XML n’émet pas une instruction COMMIT ou ROLLBACK en cas de réussite ou un échec, respectivement. En cas d'erreur, le chargement en masse XML retourne le message d'erreur approprié. La décision d'émettre une instruction COMMIT ou ROLLBACK appartient au client qui a initialisé le chargement en masse. Objet de connexion qui est utilisé pour le chargement en masse XML doit être de type ICommand ou un objet command ADO.  
  
 Dans SQLXML 4.0, un ConnectionObject ne peut pas être utilisé avec la propriété Transaction a la valeur FALSE. Le mode non transactionnel n’est pas pris en charge avec un ConnectionObject car il est impossible d’ouvrir plus d’une interface IRowsetFastLoad sur une session passée.  
  
  
