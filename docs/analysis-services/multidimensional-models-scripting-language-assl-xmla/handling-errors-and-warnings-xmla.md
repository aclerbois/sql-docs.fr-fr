---
title: Gestion des erreurs et avertissements (XMLA) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f2ba225f749364101f800efc4316b941bf18aac5
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="handling-errors-and-warnings-xmla"></a>Gestion des erreurs et des avertissements (XMLA)
  Gestion des erreurs est requises lorsqu’un document XML for Analysis (XMLA) [Discover](../../analysis-services/xmla/xml-elements-methods-discover.md) ou [Execute](../../analysis-services/xmla/xml-elements-methods-execute.md) appel de méthode ne s’exécute pas, s’exécute correctement mais génère des erreurs ou avertissements, ou s’exécute correctement mais retourne des résultats qui contiennent des erreurs.  
  
|Erreur|Signalement|  
|-----------|---------------|  
|L'appel de méthode XMLA ne s'exécute pas|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Retourne un message d’erreur SOAP qui contient les détails de l’échec.<br /><br /> Pour plus d’informations, consultez la section [gestion des erreurs SOAP](#handling_soap_faults).|  
|Erreurs ou avertissements sur un appel de méthode ayant abouti|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] inclut un [erreur](../../analysis-services/xmla/xml-elements-properties/error-element-xmla.md) ou [avertissement](../../analysis-services/xmla/xml-elements-properties/warning-element-xmla.md) , élément pour chaque erreur ou avertissement, respectivement, dans le [Messages](../../analysis-services/xmla/xml-elements-properties/messages-element-xmla.md) propriété de la [racine](../../analysis-services/xmla/xml-elements-properties/root-element-xmla.md) élément qui contient les résultats de l’appel de méthode.<br /><br /> Pour plus d’informations, consultez la section [gestion des erreurs et avertissements](#handling_errors_and_warnings).|  
|Erreurs dans les résultats d'un appel de méthode ayant abouti|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] inclut une ligne **erreur** ou **avertissement** , élément pour l’erreur ou l’avertissement, respectivement, dans les [cellule](../../analysis-services/xmla/xml-elements-properties/cell-element-xmla.md) ou [ligne](../../analysis-services/xmla/xml-elements-properties/row-element-xmla.md) élément des résultats de l’appel de méthode.<br /><br /> Pour plus d’informations, consultez la section [la gestion des erreurs et insérés avertissements](#handling_inline_errors_and_warnings).|  
  
##  <a name="handling_soap_faults"></a> Gestion des erreurs SOAP  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] retourne une erreur SOAP lorsque les situations suivantes se présentent :  
  
-   le message SOAP qui contient la méthode XMLA n'était pas correctement formé ou n'a pas pu être validé par l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ;  
  
-   une communication ou une autre erreur s'est produite impliquant le message SOAP qui contient la méthode XMLA ;  
  
-   la méthode XMLA ne s'est pas exécutée sur l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 Les codes d'erreur SOAP pour XMLstartA commencent par « XMLForAnalysis », suivis d'un point et du code de résultat hexadécimal HRESULT. Par exemple, le code d'erreur « `0x80000005` » prend la forme « `XMLForAnalysis.0x80000005` ». Pour plus d'informations sur le format d'erreur SOAP, consultez la rubrique consacrée aux erreurs Soap dans le document « Simple Object Access Protocol (SOAP) 1.1 » du W3C (en anglais).  
  
### <a name="fault-code-information"></a>Informations de code d'erreur  
 Le tableau suivant présente les informations de code d'erreur XMLA contenues dans la section détaillée de la réponse SOAP. Les colonnes représentent les attributs d'une erreur dans la section détaillée d'une erreur SOAP.  
  
|Nom de colonne|Type| Description|Null autorisé<sup>1</sup>|  
|-----------------|----------|-----------------|------------------------------|  
|**ErrorCode**|**UnsignedInt**|Code de retour qui indique le succès ou l'échec de la méthode. La valeur hexadécimale doit être convertie en un **UnsignedInt** valeur.|non|  
|**WarningCode**|**UnsignedInt**|Code de retour qui indique une condition d'avertissement. La valeur hexadécimale doit être convertie en un **UnsignedInt** valeur.|Oui|  
|**Description**|**String**|Texte et description de l'erreur ou de l'avertissement retourné par le composant qui a généré l'erreur.|Oui|  
|**Source**|**String**|Nom du composant qui a généré l'erreur ou l'avertissement.|Oui|  
|**HelpFile**|**String**|Chemin d'accès ou URL menant au fichier ou à la rubrique d'aide qui décrit l'erreur ou l'avertissement.|Oui|  
  
 <sup>1</sup> indique si les données est obligatoire et doivent être retournées, ou si les données sont facultatives et une chaîne null est autorisée si la colonne ne s’applique pas.  
  
 Voici un exemple d'erreur SOAP qui s'est produite à l'occasion d'un échec d'appel de méthode :  
  
```  
<?xml version="1.0"?>  
   <SOAP-ENV:Envelope  
   xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"  
   SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">  
      <SOAP-ENV:Fault>  
         <faultcode>XMLAnalysisError.0x80000005</faultcode>  
         <faultstring>The XML for Analysis provider encountered an error.</faultstring>  
         <faultactor>XML for Analysis Provider</faultactor>  
         <detail>  
<Error  
ErrorCode="2147483653"  
Description="An unexpected error has occurred."  
Source="XML for Analysis Provider"  
HelpFile="" />  
         </detail>  
      </SOAP-ENV:Fault>  
</SOAP-ENV:Envelope>  
```  
  
##  <a name="handling_errors_and_warnings"></a> Gestion des erreurs et avertissements  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Retourne le **Messages** propriété dans le **racine** élément pour une commande si les situations suivantes se produisent après l’exécution de cette commande :  
  
-   la méthode proprement dite n'a pas échoué, mais un échec s'est produit dans l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] après que l'appel de méthode a abouti ;  
  
-   l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] retourne un avertissement lorsque la commande aboutit.  
  
 Le **Messages** propriété suit toutes les autres propriétés qui sont contenues par le **racine** élément et peut contenir un ou plusieurs **Message** éléments. Ensuite, chaque **Message** élément peut contenir un seul **erreur** ou **avertissement** élément qui décrit les erreurs ou avertissements, respectivement, qui s’est produite pour la commande spécifiée.  
  
 Pour plus d’informations sur les erreurs et avertissements qui sont contenus dans le **Messages** propriété, consultez [élément Messages &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-properties/messages-element-xmla.md).  
  
### <a name="handling-errors-during-serialization"></a>Gestion des erreurs pendant la sérialisation  
 Si une erreur se produit après la [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance a déjà commencé à sérialiser la sortie d’une commande exécutée avec succès, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] retourne un [Exception](../../analysis-services/xmla/xml-elements-properties/exception-element-xmla.md) élément dans un espace de noms différent au moment de l’erreur. L'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ferme alors tous les éléments ouverts de telle sorte que le document XML envoyé au client soit un document valide. L’instance retourne également une **Messages** élément qui contient la description de l’erreur.  
  
##  <a name="handling_inline_errors_and_warnings"></a> La gestion des erreurs insérées et les avertissements  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Renvoie une ligne **erreur** ou **avertissement** pour une commande si la méthode XMLA proprement dite n’a pas échoué, mais une erreur spécifique à un élément de données dans les résultats retournés par la méthode s’est produite sur le [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance après que l’appel de méthode XMLA a réussi.  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit les inline **erreur** et **avertissement** éléments si les problèmes spécifiques à une cellule ou à d’autres données qui sont contenues dans un **racine** à l’aide de l’élément le [ MDDataSet](../../analysis-services/xmla/xml-data-types/mddataset-data-type-xmla.md) type de données se produisent, par exemple une erreur de sécurité ou mise en forme d’une cellule. Dans ce cas, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] retourne un **erreur** ou **avertissement** élément dans le **cellule** ou **ligne** élément contenant l’erreur ou l’avertissement, respectivement.  
  
 L’exemple suivant illustre un jeu de résultats qui contient une erreur dans l’ensemble de lignes retournée par une **Execute** à l’aide de la méthode la [instruction](../../analysis-services/xmla/xml-elements-commands/statement-element-xmla.md) commande.  
  
```  
<return>  
   ...  
   <root>  
      ...  
      <CellData>  
      ...  
         <Cell CellOrdinal="10">  
            <Value>  
               <Error>  
                  <ErrorCode>2148497527</ErrorCode>   
                  <Description>Security Error.</Description>   
               </Error>  
            </Value>  
         </Cell>  
      </CellData>  
      ...  
   </root>  
   ...  
</return>  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Développement avec XMLA dans Analysis Services](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)  
  
  
