---
title: ISSCommandWithParameters::GetParameterProperties (OLE DB) | Documents Microsoft
description: ISSCommandWithParameters::GetParameterProperties (OLE DB)
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
- ISSCommandWithParameters::GetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- GetParameterProperties method
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: c969100441cfc901ded13e7d1cdc7c551db23d42
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="isscommandwithparametersgetparameterproperties-ole-db"></a>ISSCommandWithParameters::GetParameterProperties (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Retourne un tableau de structures de jeu de propriétés SSPARAMPROPS, avec une propriété SSPARAMPROPS définie pour chaque paramètre UDT ou XML.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT GetParameterProperties(  
      DB_UPARAMS *pcParams,  
      SSPARAMPROPS **prgParamProperties);  
```  
  
## <a name="arguments"></a>Arguments  
 *pcParams*[out] [in]  
 Pointeur vers la mémoire qui contient le nombre de structures SSPARAMPROPS retournées dans *prgParamProperties*.  
  
 *prgParamProperties*[out]  
 Pointeur vers la mémoire dans laquelle est retourné un tableau de structures SSPARAMPROPS. Le fournisseur alloue de la mémoire pour les structures et retourne l’adresse à cette mémoire, le consommateur libère cette mémoire avec **IMalloc::Free** lorsqu’il n’a plus besoin les structures. Avant d’appeler **IMalloc::Free** pour *prgParamProperties*, le consommateur doit également appeler **VariantClear** pour le *vValue* propriété de chaque structure DBPROP afin d’éviter une fuite de mémoire dans les cas où la variante contient une référence de type comme une chaîne BSTR. Si *pcParams* est la valeur zéro en sortie ou une erreur autre que DB_E_ERRORSOCCURRED se produit, le fournisseur n’alloue pas de mémoire et vérifie *prgParamProperties* est un pointeur null en sortie.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Le **GetParameterProperties** méthode retourne les mêmes codes d’erreur que le noyau OLE DB **ICommandProperties::GetProperties** (méthode), sauf que DB_S_ERRORSOCCURRED et le DB_E_ERRORSOCCURED ne peut pas être déclenché.  
  
## <a name="remarks"></a>Notes  
 **ISSCommandWithParameters::GetParameterProperties** méthode se comporte de manière cohérente par rapport au **GetParameterInfo**. Si [ISSCommandWithParameters::SetParameterProperties](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md) ou **SetParameterInfo** n’ont pas été appelé ou a été appelé avec cParams égal à zéro, **GetParameterInfo**dérive les informations de paramètre et le retourne. Si **ISSCommandWithParameters::SetParameterProperties** ou **SetParameterInfo** ont été appelés pour au moins un paramètre, **ISSCommandWithParameters::GetParameterProperties**  méthode retourne les propriétés uniquement pour ces paramètres pour lesquels **ISSCommandWithParameters::SetParameterProperties** a été appelée. Si **ISSCommandWithParameters::SetParameterProperties** est appelée après **ISSCommandWithParameters::GetParameterProperties** ou **GetParameterInfo**, les appels suivants à **ISSCommandWithParameters::GetParameterProperties** retourner les valeurs substituées pour les paramètres pour lesquels **ISSCommandWithParameters::SetParameterProperties** méthode a été appelée.  
  
 La structure SSPARAMPROPS est défini comme suit :  
  
 `struct SSPARAMPROPS {`  
  
 `DBORDINAL iOrdinal;`  
  
 `ULONG cPropertySets;`  
  
 `DBPROPSET *rgPropertySets;`  
  
 `};`  
  
|Membre| Description|  
|------------|-----------------|  
|*iOrdinal*|Position ordinale du paramètre transmis.|  
|*cPropertySets*|Le nombre de structures DBPROPSET dans *rgPropertySets*.|  
|*rgPropertySets*|Pointeur vers la mémoire dans lequel retourner un tableau de structures DBPROPSET.|  
  
## <a name="see-also"></a>Voir aussi  
 [ISSCommandWithParameters & #40 ; OLE DB & #41 ;](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
