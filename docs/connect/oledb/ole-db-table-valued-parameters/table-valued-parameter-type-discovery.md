---
title: Détection de Type de paramètre table | Documents Microsoft
description: Table-Valued paramètre découverte du type à l’aide du pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-table-valued-parameters
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, type discovery
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 502db140bbfb63bf4d0c3d7c6a82bb2459633384
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="table-valued-parameter-type-discovery"></a>Découverte du type de paramètre table
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Le consommateur, autrement dit, l’application cliente utilisant le pilote OLE DB pour SQL Server — peut découvrir le type de chaque paramètre de commande si le texte de commande a été donné au fournisseur OLE DB. Une fois le type d’un paramètre table connu, le consommateur peut découvrir les informations de métadonnées de chacune des colonnes du paramètre table incluse.  
  
 Les informations de type des paramètres de procédure sont pris en charge par ICommandWithParameters::GetParameterInfo pour la plupart des types de paramètre. À partir de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], avec l’introduction des types définis par l’utilisateur et le **xml** type de données, la méthode GetParameterInfo était insuffisante à cette fin, car il n’était pas possible de fournir des informations de type défini par l’utilisateur (nom, schéma et catalogue) par le biais ICommandWithParameters. Une nouvelle interface, ISSCommandWithParameters, a été définie pour fournir des informations de type étendu.  
  
 Pour les paramètres table, vous utilisez également l’interface ISSCommandWithParameters pour découvrir des informations détaillées. Le client appelle ISSCommandWithParameters::GetParameterInfo après avoir préparé l’objet de commande. Pour les paramètres table, le *wType* membre de la structure DBPARAMINFO est défini sur DBTYPE_TABLE par le fournisseur. Le *ulParamSize* champ de la structure DBPARAMINFO a la valeur ~ 0.  
  
 Le consommateur peut ensuite demander des propriétés supplémentaires (nom de catalogue de type de paramètre table, nom de schéma de type de paramètre table, nom de type de paramètre table, colonne de classement et les colonnes par défaut) à l’aide de ISSCommandWithParamters::GetParameterProperties.  
  
 Une fois le nom de type est connu, pour récupérer les informations de colonne que le consommateur doit appeler soit IOpenRowset::OpenRowsetor obtenir l’ensemble de lignes DBSCHEMA_TABLE_TYPE_COLUMNS en spécifiant le nom de type de paramètre table comme nom de table.  
  
## <a name="see-also"></a>Voir aussi  
 [Paramètres table &#40;OLE DB&#41;](../../oledb/ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Utilisez la Table-Valued paramètres & #40 ; OLE DB & #41 ;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
