---
title: La création de lignes de paramètre table | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db-table-valued-parameters
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, rowset creation
ms.assetid: ffe213ca-cc0e-465e-b31c-a8272324c4fe
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3c7202570f46ac3ba8045ab825ddd1874f64dd6b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="table-valued-parameter-rowset-creation"></a>Création d'un ensemble de lignes de paramètre table
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Bien que les consommateurs puissent fournir n'importe quel objet d'ensemble de lignes pour les paramètres table, les objets d'ensemble de lignes communs sont implémentés dans des banques de données principales, ce qui limite leurs performances. C'est pourquoi le fournisseur OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client permet aux consommateurs de créer un objet d'ensemble de lignes spécialisé par-dessus les données en mémoire. Cet objet de l’ensemble de lignes spécial, en mémoire est un nouvel objet COM appelé un ensemble de lignes de paramètre table. Il fournit des fonctionnalités similaires aux jeux de paramètres.  
  
 Les objets d'ensemble de lignes de paramètre table sont créés explicitement par le consommateur pour les paramètres d'entrée via plusieurs interfaces de niveau session. Il existe une instance d'un objet d'ensemble de lignes de paramètre table par paramètre table. Le consommateur peut créer les objets d'ensemble de lignes de paramètre table soit en fournissant des informations de métadonnées qui sont déjà connues (scénario statique), soit en révélant ces informations par le biais des interfaces du fournisseur (scénario dynamique). Les sections qui suivent décrivent ces deux scénarios :  
  
## <a name="static-scenario"></a>Scénario statique  
 Lorsque les informations de type sont connues, le consommateur utilise ITableDefinitionWithConstraints::CreateTableWithConstraints pour instancier un objet d’ensemble de lignes de paramètre table qui correspond à un paramètre table.  
  
 Le *guid* champ (*pTableID* paramètre) contient le GUID spécial (CLSID_ROWSET_TVP). Le *pwszName* membre contient le nom du type de paramètre table que le consommateur souhaite instancier. Le *eKind* champ est défini sur DBKIND_GUID_NAME. Ce nom est requis lorsque l'instruction est une instruction SQL ad hoc ; le nom est facultatif s'il s'agit d'un appel de procédure.  
  
 Pour l’agrégation, le consommateur passe le *pUnkOuter* paramètre avec la commande de contrôle IUnknown.  
  
 Propriétés de l’objet ensemble de lignes de paramètre table sont en lecture seule, donc le consommateur n’est pas attendu pour définir les propriétés dans *rgPropertySets*.  
  
 Pour le *rgPropertySets* membre de chaque structure DBCOLUMNDESC, le consommateur peut spécifier des propriétés supplémentaires pour chaque colonne. Ces propriétés appartiennent au jeu de propriétés DBPROPSET_SQLSERVERCOLUMN. Elles vous permettent de spécifier les paramètres calculés et les paramètres par défaut de chaque colonne. Elles prennent également en charge les propriétés de colonnes existantes, telles que la possibilité de valeur Null et l'identité.  
  
 Pour récupérer les informations correspondantes à partir d’un objet d’ensemble de lignes de paramètre table, le consommateur utilise IRowsetInfo::GetProperties.  
  
 Pour récupérer des informations sur la valeur null, unique, calculée et mettre à jour l’état de chaque colonne, le consommateur utiliser IColumnsRowset::GetColumnsRowset ou IColumnsInfo::GetColumnInfo. Ces méthodes fournissent des informations détaillées sur chaque colonne de l'ensemble de lignes du paramètre table.  
  
 Le consommateur spécifie le type de chaque colonne du paramètre table. Cela est identique à la spécification de colonnes lors de la création d'une table dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le consommateur Obtient un objet d’ensemble de lignes de paramètre table à partir de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur Native Client OLE DB via la *ppRowset* paramètre de sortie.  
  
## <a name="dynamic-scenario"></a>Scénario Dynamique  
 Lorsque le consommateur n’a pas d’informations de type, elle doit utiliser IOpenRowset::OpenRowset pour instancier des objets d’ensemble de lignes de paramètre table. Tout ce que le consommateur doit fournir au fournisseur est le nom du type.  
  
 Dans ce scénario, le fournisseur obtient les informations de type d'un objet d'ensemble de lignes de paramètre table à partir du serveur au nom du consommateur.  
  
 Le *pTableID* et *pUnkOuter* paramètres doivent être définis que dans le scénario statique. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur Native Client OLE DB puis obtient les informations de type (informations sur les colonnes et contraintes) à partir du serveur et retourner un objet d’ensemble de lignes de paramètre table via le *ppRowset* paramètre. Cette opération requiert une communication avec le serveur, et par conséquent ne fonctionne pas aussi bien que le scénario statique. Le scénario dynamique fonctionne uniquement avec des appels de procédure paramétrables.  
  
## <a name="see-also"></a>Voir aussi  
 [Paramètres table &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Utilisez la Table-Valued paramètres & #40 ; OLE DB & #41 ;](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
