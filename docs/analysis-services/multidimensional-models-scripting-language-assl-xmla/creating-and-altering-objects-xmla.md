---
title: Création et modification d’objets (XMLA) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: fcfb8d2f7ec13c4dda5aa533096d3ff5d5820fe1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="creating-and-altering-objects-xmla"></a>Création et modification d'objets (XMLA)
  Les objets principaux peuvent être créés, modifiés et supprimés de manière indépendante. Les objets principaux se composent notamment des objets suivants :  
  
-   Serveurs  
  
-   Bases de données  
  
-   Dimensions  
  
-   Cubes  
  
-   les groupes de mesures ;  
  
-   Partitions  
  
-   Perspectives  
  
-   Modèles d'exploration de données  
  
-   Rôles  
  
-   Commandes associées à un serveur ou à une base de données  
  
-   Sources de données  
  
 Vous utilisez la [créer](../../analysis-services/xmla/xml-elements-commands/create-element-xmla.md) commande pour créer un objet principal sur une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]et le [Alter](../../analysis-services/xmla/xml-elements-commands/alter-element-xmla.md) commande pour modifier un objet principal existant sur une instance. Les deux commandes sont exécutées à l’aide de la [Execute](../../analysis-services/xmla/xml-elements-methods-execute.md) (méthode).  
  
## <a name="creating-objects"></a>Création d’objets  
 Lorsque vous créez des objets à l’aide de la **créer** (méthode), vous devez d’abord identifier l’objet parent qui contient le [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] objet à créer. Identification de l’objet parent en fournissant une référence d’objet dans le [ParentObject](../../analysis-services/xmla/xml-elements-properties/parentobject-element-xmla.md) propriété de la **créer** commande. Chaque référence d’objet contient les identificateurs d’objet nécessaires pour identifier de façon unique l’objet parent pour le **créer** commande. Pour plus d’informations sur les références d’objet, consultez [définition et identification d’objets &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/defining-and-identifying-objects-xmla.md).  
  
 Par exemple, vous devez fournir une référence d'objet à un cube pour créer un groupe de mesures pour le cube. La référence d’objet pour le cube dans le **ParentObject** propriété contient un identificateur de la base de données et un identificateur de cube, comme le même identificateur de cube pourrait être utilisé sur une autre base de données.  
  
 Le [ObjectDefinition](../../analysis-services/xmla/xml-elements-properties/objectdefinition-element-xmla.md) élément contient des éléments Analysis Services Scripting Language (ASSL) qui définissent l’objet principal doit être créé. Pour plus d’informations sur ASSL, consultez [développement avec Analysis Services Scripting Language &#40;ASSL&#41;](../../analysis-services/multidimensional-models/scripting-language-assl/developing-with-analysis-services-scripting-language-assl.md).  
  
 Si vous définissez la **AllowOverwrite** attribut de la **créer** commande sur true, vous pouvez remplacer un objet principal existant ayant l’identificateur spécifié. Sinon, une erreur se produit si un objet principal associé à l'identificateur spécifié existe déjà dans l'objet parent.  
  
 Pour plus d’informations sur la **créer** command, consultez [créer un élément &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/create-element-xmla.md).  
  
### <a name="creating-session-objects"></a>Création d'objets de session  
 Les objets de session sont des objets temporaires disponibles uniquement pour la session explicite ou implicite utilisée par une application cliente et qui sont supprimés à la fin de la session. Vous pouvez créer des objets de session en définissant le **étendue** attribut de la **créer** commande *Session*.  
  
> [!NOTE]  
>  Lorsque vous utilisez la *Session* définition, le **ObjectDefinition** élément ne peut contenir [Dimension](../../analysis-services/scripting/objects/dimension-element-assl.md), [Cube](../../analysis-services/scripting/objects/cube-element-assl.md), ou [MiningModel](../../analysis-services/scripting/objects/miningmodel-element-assl.md) éléments ASSL.  
  
## <a name="altering-objects"></a>Modification d'objets  
 Lorsque vous modifiez des objets à l’aide de la **Alter** (méthode), vous devez d’abord identifier l’objet à modifier en fournissant une référence d’objet dans le [objet](../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md) propriété de la **Alter** commande. Chaque référence d’objet contient les identificateurs d’objet nécessaires pour identifier de façon unique l’objet pour le **Alter** commande. Pour plus d’informations sur les références d’objet, consultez [définition et identification d’objets &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/defining-and-identifying-objects-xmla.md).  
  
 Par exemple, vous devez fournir une référence d'objet à un cube pour modifier la structure de ce dernier. La référence d’objet pour le cube dans le **objet** propriété contient un identificateur de la base de données et un identificateur de cube, comme le même identificateur de cube pourrait être utilisé sur une autre base de données.  
  
 Le **ObjectDefinition** élément contient des éléments ASSL qui définissent l’objet principal à modifier. Pour plus d’informations sur ASSL, consultez [développement avec Analysis Services Scripting Language &#40;ASSL&#41;](../../analysis-services/multidimensional-models/scripting-language-assl/developing-with-analysis-services-scripting-language-assl.md).  
  
 Si vous définissez la **AllowCreate** attribut de la **Alter** commande sur true, vous pouvez créer l’objet principal spécifié si l’objet n’existe pas. Sinon, une erreur se produit si un objet principal spécifié n'existe pas déjà.  
  
### <a name="using-the-objectexpansion-attribute"></a>Utilisation de l'attribut ObjectExpansion  
 Si vous modifiez uniquement les propriétés de l’objet principal et que vous ne redéfinissez pas les objets secondaires contenus dans l’objet principal, vous pouvez définir le **ObjectExpansion** attribut de la **Alter** commande *ObjectProperties*. Le **ObjectDefinition** propriété ne doit donc contenir les éléments pour les propriétés de l’objet principal et le **Alter** commande laisse les objets secondaires associés à l’objet principal inchangés.  
  
 Pour redéfinir les objets secondaires pour un objet principal, vous devez définir le **ObjectExpansion** attribut *ExpandFull* et la définition d’objet doit inclure tous les objets secondaires contenus dans l’objet principal. Si le **ObjectDefinition** propriété de la **Alter** commande n’inclut pas explicitement un objet secondaire qui est contenu dans l’objet principal, l’objet secondaire qui n’a pas été inclus est supprimé.  
  
### <a name="altering-session-objects"></a>Modification d'objets de session  
 Pour modifier les objets de session créés par le **créer** commande, définissez la **étendue** attribut de la **Alter** commande *Session*.  
  
> [!NOTE]  
>  Lorsque vous utilisez la *Session* définition, le **ObjectDefinition** élément ne peut contenir [Dimension](../../analysis-services/scripting/objects/dimension-element-assl.md), [Cube](../../analysis-services/scripting/objects/cube-element-assl.md), ou [MiningModel](../../analysis-services/scripting/objects/miningmodel-element-assl.md) éléments ASSL.  
  
## <a name="creating-or-altering-subordinate-objects"></a>Création ou modification d'objets subordonnés  
 Bien qu’un **créer** ou **Alter** commande crée ou modifie un seul objet principales au premier plan, l’objet principal ainsi créé ou modifié peut contenir des définitions dans la forme **ObjectDefinition** propriété pour d’autres objets principaux et secondaires qui lui sont subordonnés. Par exemple, si vous définissez un cube, vous spécifiez la base de données parente dans **ParentObject**et dans la définition de cube **ObjectDefinition** vous pouvez définir des groupes de mesures du cube, et dans les groupes de mesures, vous pouvez définir des partitions pour chaque groupe de mesures. Un objet secondaire ne peut être défini que sous l'objet principal qui le contient. Pour plus d’informations sur les objets principaux et secondaires, consultez [des objets de base de données &#40;Analysis Services - données multidimensionnelles&#41;](../../analysis-services/multidimensional-models/olap-logical/database-objects-analysis-services-multidimensional-data.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="description"></a> Description  
 L’exemple suivant crée une source de données relationnelle qui fait référence à la [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)] exemple [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données.  
  
### <a name="code"></a>Code  
  
```  
<Create xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
    <ParentObject>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
    </ParentObject>  
    <ObjectDefinition>  
        <DataSource xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="RelationalDataSource">  
            <ID>AdventureWorksDW2012</ID>  
            <Name>AdventureWorksDW2012</Name>  
            <ConnectionString>Data Source=localhost;Initial Catalog=AdventureWorksDW2008R2;Integrated Security=True</ConnectionString>  
            <ImpersonationInfo>  
                <ImpersonationMode>ImpersonateServiceAccount</ImpersonationMode>  
            </ImpersonationInfo>  
            <ManagedProvider>System.Data.SqlClient</ManagedProvider>  
            <Timeout>PT0S</Timeout>  
        </DataSource>  
    </ObjectDefinition>  
</Create>  
```  
  
### <a name="description"></a> Description  
 L'exemple suivant modifie la source de données relationnelle créée dans l'exemple précédent pour définir le délai d'expiration de requête de la source de données à 30 secondes.  
  
### <a name="code"></a>Code  
  
```  
<Alter ObjectExpansion="ObjectProperties" xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
    <Object>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
        <DataSourceID>AdventureWorksDW2012</DataSourceID>  
    </Object>  
    <ObjectDefinition>  
        <DataSource xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="RelationalDataSource">  
            <ID>AdventureWorksDW2012</ID>  
            <Name>AdventureWorksDW2012</Name>  
            <ConnectionString>Data Source=fr-dwk-02;Initial Catalog=AdventureWorksDW2008R2;Integrated Security=True</ConnectionString>  
            <ManagedProvider>System.Data.SqlClient</ManagedProvider>  
            <Timeout>PT30S</Timeout>  
        </DataSource>  
    </ObjectDefinition>  
</Alter>  
```  
  
### <a name="comments"></a>Commentaires  
 Le **ObjectExpansion** attribut de la **Alter** commande a été définie sur *ObjectProperties*. Ce paramètre permet la [ImpersonationInfo](../../analysis-services/scripting/properties/impersonationinfo-element-assl.md) élément, un objet secondaire, à exclure de la source de données définie dans **ObjectDefinition**. Par conséquent, les informations d'emprunt d'identité de cette source de données restent définies sur le compte de service, comme spécifié dans le premier exemple.  
  
## <a name="see-also"></a>Voir aussi  
 [Exécuter la méthode &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-methods-execute.md)   
 [Développement avec Analysis Services Scripting Language &#40;ASSL&#41;](../../analysis-services/multidimensional-models/scripting-language-assl/developing-with-analysis-services-scripting-language-assl.md)   
 [Développement avec XMLA dans Analysis Services](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)  
  
  
