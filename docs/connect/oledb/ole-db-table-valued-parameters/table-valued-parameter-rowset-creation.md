---
title: La création de lignes de paramètre table | Documents Microsoft
description: Création de jeu de lignes de paramètre table statique et dynamique
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
- table-valued parameters, rowset creation
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 0bfd6f54e374c77268641e151f738efa8a2385b5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="table-valued-parameter-rowset-creation"></a>Création d'un ensemble de lignes de paramètre table
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Bien que les consommateurs puissent fournir n'importe quel objet d'ensemble de lignes pour les paramètres table, les objets d'ensemble de lignes communs sont implémentés dans des banques de données principales, ce qui limite leurs performances. Pour cette raison, le pilote OLE DB pour SQL Server permet aux consommateurs de créer un objet d’ensemble de lignes spécialisé par-dessus les données en mémoire. Cet objet d'ensemble de lignes spécial stocké en mémoire est un nouvel objet COM appelé un ensemble de lignes de paramètre table. Il fournit des fonctionnalités similaires aux jeux de paramètres.  
  
 Les objets d'ensemble de lignes de paramètre table sont créés explicitement par le consommateur pour les paramètres d'entrée via plusieurs interfaces de niveau session. Il existe une seule instance d’un objet d’ensemble de lignes de paramètre table par paramètre table. Le consommateur peut créer les objets d'ensemble de lignes de paramètre table soit en fournissant des informations de métadonnées qui sont déjà connues (scénario statique), soit en révélant ces informations par le biais des interfaces du fournisseur (scénario dynamique). Les sections qui suivent décrivent ces deux scénarios :  
  
## <a name="static-scenario"></a>Scénario statique  
 Lorsque les informations de type sont connues, le consommateur utilise ITableDefinitionWithConstraints::CreateTableWithConstraints pour instancier un objet d’ensemble de lignes de paramètre table qui correspond à un paramètre table.  
  
 Le *guid* champ (*pTableID* paramètre) contient le GUID spécial (CLSID_ROWSET_TVP). Le *pwszName* membre contient le nom du type de paramètre table que le consommateur souhaite instancier. Le *eKind* champ est défini sur DBKIND_GUID_NAME. Ce nom est requis lors de l’instruction SQL ad hoc ; le nom est facultatif s’il s’agit d’un appel de procédure.  
  
 Pour l’agrégation, le consommateur passe le *pUnkOuter* paramètre avec la commande de contrôle IUnknown.  
  
 Propriétés de l’objet ensemble de lignes de paramètre table sont en lecture seule, donc le consommateur n’est pas prévu pour définir les propriétés dans *rgPropertySets*.  
  
 Pour le *rgPropertySets* membre de chaque structure DBCOLUMNDESC, le consommateur peut spécifier des propriétés supplémentaires pour chaque colonne. Ces propriétés appartiennent au jeu de propriétés DBPROPSET_SQLSERVERCOLUMN. Elles vous permettent de spécifier les paramètres calculés et les paramètres par défaut de chaque colonne. Elles prennent également en charge les propriétés de colonnes existantes, telles que la possibilité de valeur Null et l'identité.  
  
 Pour récupérer les informations correspondantes à partir d’un objet d’ensemble de lignes de paramètre table, le consommateur utilise IRowsetInfo::GetProperties.  
  
 Pour récupérer des informations sur la valeur null, unique, calculée et mettre à jour l’état de chaque colonne, le consommateur peut utiliser IColumnsRowset::GetColumnsRowset ou IColumnsInfo::GetColumnInfo. Ces méthodes fournissent des informations détaillées sur chaque colonne de l'ensemble de lignes du paramètre table.  
  
 Le consommateur spécifie le type de chaque colonne du paramètre table. Il est similaire à la façon dont les colonnes sont spécifiées lors de la création d’une table dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le consommateur Obtient un objet d’ensemble de lignes de paramètre table à partir du pilote OLE DB pour SQL Server via le *ppRowset* paramètre de sortie.  
  
## <a name="dynamic-scenario"></a>Scénario Dynamique  
 Lorsque le consommateur ne dispose pas des informations de type, elle doit utiliser IOpenRowset::OpenRowset pour instancier des objets d’ensemble de lignes de paramètre table. Tout ce que le consommateur doit fournir au fournisseur est le nom du type.  
  
 Dans ce scénario, le fournisseur obtient les informations de type d'un objet d'ensemble de lignes de paramètre table à partir du serveur au nom du consommateur.  
  
 Le *pTableID* et *pUnkOuter* paramètres doivent être définis que dans le scénario statique. Le pilote OLE DB pour SQL Server puis obtient les informations de type (informations sur les colonnes et contraintes) à partir du serveur et retourner un objet d’ensemble de lignes de paramètre table via le *ppRowset* paramètre. Cette opération requiert une communication avec le serveur et ne peut donc pas exécuter, ainsi que le scénario statique. Le scénario dynamique fonctionne uniquement avec des appels de procédure paramétrables.  
  
## <a name="see-also"></a>Voir aussi  
 [Paramètres table &#40;OLE DB&#41;](../../oledb/ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Utilisez la Table-Valued paramètres & #40 ; OLE DB & #41 ;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
