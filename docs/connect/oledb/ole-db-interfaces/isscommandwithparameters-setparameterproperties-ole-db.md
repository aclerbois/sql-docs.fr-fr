---
title: ISSCommandWithParameters::SetParameterProperties (OLE DB) | Documents Microsoft
description: ISSCommandWithParameters::SetParameterProperties (OLE DB)
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-interfaces
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- ISSCommandWithParameters::SetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- SetParameterProperties method
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: d10258b631c5ed6852d940682815793497ffb7a1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="isscommandwithparameterssetparameterproperties-ole-db"></a>ISSCommandWithParameters::SetParameterProperties (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Définit les propriétés de paramètre pour chaque paramètre par ordinal ou définit des propriétés de paramètre en bloc en spécifiant un tableau de structures SSPARAMPROPS.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT SetParameterProperties(  
      DB_UPARAMS cParams,   
      SSPARAMPROPS rgParamProperties[]);  
```  
  
## <a name="arguments"></a>Arguments  
 *cParams*[in]  
 Le nombre de structures SSPARAMPROPS dans le *rgParamProperties* tableau. Si ce nombre est égal à zéro, **ISSCommandWithParameters::SetParameterProperties** supprimera toutes les propriétés qui ont peuvent être spécifiées pour les paramètres de la commande.  
  
 *rgParamProperties*[in]  
 Tableau de structures SSPARAMPROPS à définir.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Le **ISSCommandWithParameters::SetParameterProperties** méthode retourne les mêmes codes d’erreur que le noyau OLE DB **ICommandProperties::SetProperties** (méthode).  
  
## <a name="remarks"></a>Notes  
 Définition des propriétés de paramètre avec cette méthode est autorisée sur chaque paramètre par ordinal ou avec une seule **ISSCommandWithParameters::SetParameterProperties** appeler une fois SSPARAMPROPS a été généré à partir du tableau de la propriété.  
  
 Le **SetParameterInfo** méthode doit être appelée avant d’appeler le **ISSCommandWithParameters::SetParameterProperties** (méthode). Le fait d'appeler `SetParameterProperties(0, NULL)` efface toutes les propriétés de paramètre spécifiées, tandis que l'appel de `SetParameterInfo(0,NULL,NULL)` efface toutes les informations sur les paramètres y compris toutes les propriétés qui peuvent être associées à un paramètre.  
  
 Appel de **ISSCommandWithParameters::SetParameterProperties** pour spécifier les propriétés d’un paramètre qui n’est pas de type DBTYPE_XML ou DBTYPE_UDT retourne DB_E_ERRORSOCCURRED ou DB_S_ERRORSOCCURRED et marque le  *dwStatus* champ de tous les Dbprop contenus dans SSPARAMPROPS pour ce paramètre avec DBPROPSTATUS_NOTSET. Le tableau DBPROP de chaque DBPROPSET contenu dans SSPARAMPROPS doit être parcouru pour détecter le paramètre auquel DB_E_ERRORSOCCURRED ou DB_S_ERRORSOCCURRED fait référence.  
  
 Si **ISSCommandWithParameters::SetParameterProperties** est appelée pour spécifier les propriétés de paramètres dont les informations de paramètre n'ont pas encore été définies avec **SetParameterInfo**, le fournisseur retourne E_UNEXPECTED avec le message d’erreur suivant :  
  
 La méthode SetParameterProperties ne peut pas être appelée pour les paramètres spécifiés sans appeler auparavant la méthode SetParameterInfo. Vous devez définir les informations de paramètre avant de définir les propriétés de paramètre.  
  
 Si l’appel à **ISSCommandWithParameters::SetParameterProperties** contient des paramètres où les informations de paramètre a été définie, et certains paramètres où les informations de paramètre n’a pas été définie, les propriétés dwStatus dans le DBPROPSET du jeu de propriétés SSPARAMPROPS seront retournées avec DBSTATUS_NOTSET.  
  
 La structure SSPARAMPROPS est défini comme suit :  
  
 `struct SSPARAMPROPS {`  
  
 `DBORDINAL iOrdinal;`  
  
 `ULONG cPropertySets;`  
  
 `DBPROPSET *rgPropertySets;`  
  
 `};`  
  
 Améliorations dans le moteur de base de données en commençant par [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] autoriser ISSCommandWithParameters::SetParameterProperties obtenir des descriptions plus exactes des résultats attendus. Ces résultats plus précis peuvent différer des valeurs retournées par ISSCommandWithParameters::SetParameterProperties dans les versions précédentes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Metadata Discovery](../../oledb/features/metadata-discovery.md).  
  
|Membre| Description|  
|------------|-----------------|  
|*iOrdinal*|Position ordinale du paramètre transmis.|  
|*cPropertySets*|Le nombre de structures DBPROPSET dans *rgPropertySets*.|  
|*rgPropertySets*|Pointeur vers la mémoire dans lequel retourner un tableau de structures DBPROPSET.|  
  
## <a name="see-also"></a>Voir aussi  
 [ISSCommandWithParameters & #40 ; OLE DB & #41 ;](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
