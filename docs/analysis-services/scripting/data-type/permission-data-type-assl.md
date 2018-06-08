---
title: Type de données d’autorisation (ASSL) | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: assl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7a7e400a8fbdca47ff678e8b02c89064771c9450
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="permission-data-type-assl"></a>Type de données Permission (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Définit un type de données primitif abstrait qui fournit des informations sur une autorisation individuelle.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Permission>  
   <Name>...</Name>  
   <ID>...</ID>  
   <CreatedTimestamp>...</CreateTimestamp>  
   <LastSchemaUpdate>...</LastSchemaUpdate>  
   <RoleID>...</RoleID>  
   <Description>...</Description>  
   <Process>...</Process>  
   <ReadDefinition>...</ReadDefinition>  
   <Read>...</Read>  
   <Write>...</Write>  
   <Annotations>...</Annotations>  
</Permission>  
```  
  
## <a name="data-type-characteristics"></a>Caractéristiques du type de données  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Types de données de base|Aucune|  
|Types de données dérivés|[CubePermission](../../../analysis-services/scripting/objects/cubepermission-element-assl.md), [DatabasePermission](../../../analysis-services/scripting/objects/databasepermission-element-assl.md), [DimensionPermission](../../../analysis-services/scripting/data-type/dimensionpermission-data-type-assl.md), [MiningModelPermission](../../../analysis-services/scripting/objects/miningmodelpermission-element-assl.md), [MiningStructurePermission](../../../analysis-services/scripting/objects/miningstructurepermission-element-assl.md)|  
  
## <a name="data-type-relationships"></a>Relations du type de données  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|Aucune|  
|Éléments enfants|[Annotations](../../../analysis-services/scripting/collections/annotations-element-assl.md), [CreatedTimestamp](../../../analysis-services/scripting/properties/createdtimestamp-element-assl.md), [Description](../../../analysis-services/scripting/properties/description-element-assl.md), [ID](../../../analysis-services/scripting/properties/id-element-assl.md), [LastSchemaUpdate](../../../analysis-services/scripting/properties/lastschemaupdate-element-assl.md), [nom](../../../analysis-services/scripting/properties/name-element-assl.md), [processus](../../../analysis-services/scripting/properties/process-element-assl.md), [en lecture](../../../analysis-services/scripting/properties/read-element-assl.md), [ReadDefinition](../../../analysis-services/scripting/properties/readdefinition-element-assl.md), [RoleID](../../../analysis-services/scripting/properties/roleid-element-assl.md), [écrire](../../../analysis-services/scripting/properties/write-element-assl.md)|  
|Éléments dérivés|Aucune|  
  
## <a name="remarks"></a>Notes  
 **Autorisation** sert comme type de base abstrait pour un nombre de types d’autorisation dérivés utilisés dans une instance de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
 Ce type de données a les validations suivantes sous la valeur 2 de DeploymentMode (mode de serveur tabulaire).  
  
-   *Processus* attribut par défaut a la valeur **False**, sauf si l’utilisateur a le **Actualiser** autorisation. Pour les utilisateurs avec le **Actualiser** autorisation le *processus* attribut a la valeur **True**.  
  
-   *ReadDefinition* attribut a la valeur **aucun**; toute autre valeur génère une erreur.  
  
-   *En lecture* attribut a la valeur **autorisées** pour les utilisateurs avec le **utilisateur** autorisation et **aucun** lorsque les utilisateurs sont affectés à la  **Actualiser** autorisation ; si un utilisateur possède à la fois **utilisateur** et **Actualiser** autorisations, puis l’attribut a la valeur **autorisées**. Pour les utilisateurs avec des privilèges d’administrateur, la valeur d’attribut est définie **autorisées**.  
  
-   *Écrire* attribut a la valeur **aucun**; toute autre valeur génère une erreur.  
  
 L’élément correspondant dans le modèle d’objet objets AMO (Analysis Management) est <xref:Microsoft.AnalysisServices.Permission>.  
  
## <a name="see-also"></a>Voir aussi  
 [Élément role & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/objects/role-element-assl.md)   
 [Analysis Services script des Types de données XML Language & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/data-type/analysis-services-scripting-language-xml-data-types-assl.md)  
  
  
